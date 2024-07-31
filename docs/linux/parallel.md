---
tags: [linux, parallel, command]
---
# Parallel

## 1. Introduction

There are many common tasks in Linux that we may want to consider running in parallel, such as:

- Downloading a large number of files
- Encoding/decoding a large number of images on a machine with multiple CPU cores
- Making a computation with many different parameters and storing the results

Of course, we can accomplish all these tasks without using parallelization. But if we process each file, connection, or **computation in several parallel processes, we can have a great advantage in terms of speed**. Luckily, there are multiple powerful command-line tools for parallelization in Linux systems that can help us achieve this.

In this tutorial, we’re going to see how to use the Bash ampersand _&_ operator, [_xargs_](https://man7.org/linux/man-pages/man1/xargs.1.html), and [GNU _parallel_](https://www.gnu.org/software/parallel/man.html) to achieve parallelization on the Linux command line.

## 2. A Sample Task

First, let’s create a simple script that we’ll run in parallel.

Let’s create a file named _./process_ with contents:

```bash
#!/bin/bash

echo "started processing $*.."
sleep $((2 + RANDOM % 3)); echo finished processing "$*";
```

This script will fake an actual process that takes 2 to 5 seconds to complete. Let’s make it executable to be able to use it:

```bash
chmod +x ./process
```

## 3. Using &

As a basic way to run commands in parallel, we can use the built-in [Bash ampersand _&_ operator](https://www.baeldung.com/linux/run-multiple-commands-in-background) to run a command asynchronously so that the shell doesn’t wait for the current command to complete before moving on to the next one:

```bash
./process 1 &
./process 2 &
```

This will create two processes that will start at essentially the same instant and run in parallel. Because we’ve introduced random _sleep_ times in our example script, the output may look like this:

```bash
[1] 25254
[2] 25255
started processing 1..
started processing 2..
finished processing 2
finished processing 1

[1]-  Done                    ./process 1
[2]+  Done                    ./process 2
```

**Clearly, we can use this approach to run many parallel processes. But if we have many tasks – for example, a hundred images to be converted – we wouldn’t want to start all hundred tasks at once, but instead, process them in batches to utilize our cores better.** To achieve this, we need to wait for some tasks to complete before starting others.

### 3.1. Using _wait_ with &

The [_wait_](https://man7.org/linux/man-pages/man2/waitid.2.html) command will, by default, wait for all child processes to exit. So, using the _wait_ command, we can run batches of operations:

AD

```bash
echo "starting batch 1.."
./process 1.jpg &
./process 2.jpg &
./process 3.jpg &
wait
echo "starting batch 2.."
./process 4.jpg &
./process 5.jpg &
./process 6.jpg &
wait
echo "finished"
```

**However, there’s one big downside to this approach. To utilize our CPU cores effectively, we’d want a new process to start as soon as a running process ends.** But with this solution, we wouldn’t start new processes until all the tasks in the previous batch were completed. To overcome this limitation, we can use _xargs_.

## 4. Using _xargs

[_xargs_ is a command-line tool](https://www.baeldung.com/linux/xargs) that helps us run commands with arguments parsed from standard input. It can also parallelize our tasks for us.

Let’s try the previous input we used with &, but this time with _xargs_:

```bash
$ echo '1.jpg 2.jpg 3.jpg 4.jpg 5.jpg 6.jpg' | xargs -n 1 -P 3 ./process
started processing 1.jpg..
started processing 2.jpg..
started processing 3.jpg..
finished processing 1.jpg
finished processing 2.jpg
started processing 4.jpg..
started processing 5.jpg..
finished processing 3.jpg
started processing 6.jpg..
finished processing 5.jpg
finished processing 4.jpg
finished processing 6.jpg
```

**_xargs_ immediately creates the next process once a process is completed.** We specify the number of arguments per call using the _-n_ argument and the number of parallel tasks using the _-P_ argument.

### 4.1. Using Replacement

If the executable we’re using requires us to put the arguments to some specific place rather than appending them directly after the executable name, we can use replacement.

Let’s try it:

```bash
$ args="1\n2\n3\n4\n5\n6"
$ echo -e $args | xargs -I "{}" -P 2 ./process {}.jpg
started processing 1.jpg..
started processing 2.jpg..
finished processing 2.jpg
started processing 3.jpg..
finished processing 1.jpg
started processing 4.jpg..
finished processing 3.jpg
started processing 5.jpg..
finished processing 5.jpg
started processing 6.jpg..
finished processing 4.jpg
finished processing 6.jpg
```

### 4.2. Handling Arguments With Newlines

If the arguments we want to use with our processes include newline characters, we can use a null character (\0) delimited input stream. For example, with the [_find_](https://www.baeldung.com/linux/find-command) command, we can set the output to be null-delimited instead of newline-delimited by using the _-print0_ flag:

```bash
find . -print0 | xargs -0 -n 2 -P 2 ./process
```

As the arguments are now null-delimited, we can be sure that newline characters in the input will be preserved.

## 5. Using GNU _parallel_

GNU _parallel_ is one of the most advanced command-line tools available for running parallel tasks. It has many features, including the ability to distribute and run tasks remotely on multiple machines using _ssh_.

### 5.1. Basic Usage

The basic usage of _parallel_ is very similar to _xargs_. Actually, for simple cases, we can use it interchangeably with _xargs_.

Let’s try:

```bash
$ args="1\n2\n3\n4\n5\n6"
$ echo -e $args | parallel --ungroup --jobs 3 ./process
started processing 1..
started processing 2..
started processing 3..
finished processing 1
finished processing 2
finished processing 3
started processing 4..
started processing 5..
started processing 6..
finished processing 5
finished processing 4
finished processing 6
```

The _–jobs_ argument is the same as the _xargs_ command’s _-P_ argument, which determines the maximum number of parallel jobs to be running at the same time.

By default, _parallel_ will print the output of a process only after it is finished. The _–ungroup_ flag disables this functionality. We can use it to see the actual execution order of commands as they are running.

We can supply the input arguments also via the command line. Let’s try running it to get the same output as above:

```bash
parallel --ungroup --jobs 3 ./process ::: 1 2 3 4 5 6
```

when supplying command-line arguments, we can use ::: (three colons) to supply arguments directly, and :::: (four colons) to supply arguments from a file.

Let’s see an example that supplies input from a file:

```bash
args="1\n2\n3\n4\n5\n6"
echo -e $args > input.txt
parallel --ungroup --jobs 3 ./process :::: input.txt
```

The output would be similar to the above.

### 5.2. Running Combinations of Multiple Sources

We can use _parallel_ to run tasks for every possible combination of two sources.

Let’s try it for two sample sources:

```bash
$ parallel --ungroup ./process ::: 1 2 3 ::: 1 2 3
started processing 1 1..
started processing 1 2..
started processing 1 3..
started processing 2 1..
started processing 2 2..
started processing 2 3..
started processing 3 1..
started processing 3 2..
finished processing 1 1
finished processing 1 2
finished processing 2 1
finished processing 2 2
finished processing 3 1
started processing 3 3..
finished processing 1 3
finished processing 3 2
finished processing 2 3
finished processing 3 3
```

### 5.3. Linking Sources

If instead of running for every possible combination, we want to “link” them after one another, we would use the _–link_ flag. Let’s try it with two different input sources:

```bash
$ parallel --link --ungroup ./process ::: 1 2 3 ::: 1 2 3
started processing 1 1..
started processing 2 2..
started processing 3 3..
finished processing 1 1
finished processing 3 3
finished processing 2 2
```

### 5.4. Replacement Strings

Like in _xargs_, we can use replacement strings in _parallel_. The default replacement string is {}.

Let’s try it with a prefix:

```bash
$ parallel --ungroup ./process item-{} ::: 1 2 3
started processing item-1..
started processing item-2..
started processing item-3..
finished processing item-1
finished processing item-2
finished processing item-3
```

Other replacement strings do different kinds of manipulations on the input. For example, {.} will remove the extension from the argument:

```bash
$ parallel --ungroup ./process {.} ::: 1.jpg 2.jpg 3.jpg
started processing 1..
started processing 2..
started processing 3..
finished processing 2
finished processing 1
finished processing 3
```

If we want to use multiple different variables for each command, we can also do this using special replacement strings:

```bash
$ parallel --ungroup --link ./process {1}.jpg {2}.jpg {3}.jpg ::: 1 2 3 ::: 4 5 6 ::: 7 8 9
started processing 1.jpg 4.jpg 7.jpg..
started processing 2.jpg 5.jpg 8.jpg..
started processing 3.jpg 6.jpg 9.jpg..
finished processing 1.jpg 4.jpg 7.jpg
finished processing 2.jpg 5.jpg 8.jpg
finished processing 3.jpg 6.jpg 9.jpg
```

There are also many more options for replacement strings that can be found in the [_parallel_ tutorial](https://www.gnu.org/software/parallel/parallel_tutorial.html).

### 5.5. Reading Input From File Columns

We can read the input from different columns of a text file. Let’s try it with a tab-separated text file:

```bash
$ args="1\t4\n2\t5\n3\t6"
$ echo -e $args > input_cols.txt
$ parallel --colsep '\t' --ungroup ./process [{1}] [{2}] :::: input_cols.txt
started processing [1] [4]..
started processing [2] [5]..
started processing [3] [6]..
finished processing [2] [5]
finished processing [3] [6]
finished processing [1] [4]
```

### 5.6. Saving Output

We can save the output of each process into a file by using the _–files_ flag:

```bash
$ parallel --files --link ./process ::: 1 2 3 ::: 1 2 3
/tmp/parnqFzp.par
/tmp/parSv0nW.par
/tmp/parGyNbz.par
```

This will create _*.par_ files with the output of our commands as the content.

If we want to have a more friendly directory structure, we can use the _–results_ and _–header_ arguments to write the results to a folder in a hierarchy.

Let’s run a command to generate the directory tree:

```bash
parallel --results outdir --link ./process ::: a b c ::: d e f
```

Now, let’s check the output using the [_tree_](https://www.baeldung.com/linux/print-tree-directory-structure) command:

```bash
outdir
└── 1
    ├── a
    │   └── 2
    │       └── d
    │           ├── seq
    │           ├── stderr
    │           └── stdout
    ├── b
    │   └── 2
    │       └── e
    │           ├── seq
    │           ├── stderr
    │           └── stdout
    └── c
        └── 2
            └── f
                ├── seq
                ├── stderr
                └── stdout
```

_parallel_ generates the directory structure based on argument positions and values.

### 5.7. Progress Information

We can also have _parallel_ show an estimate of the remaining time based on current task runs:

```bash
$ parallel --eta --colsep '\t' --ungroup ./process [{1}] [{2}] :::: input_cols.txt

started processing [1] [4]..
started processing [2] [5]..

Computers / CPU cores / Max jobs to run
1:local / 8 / 3
started processing [3] [6]..

Computer:jobs running/jobs completed/%of started jobs/Average seconds to complete
ETA: 0s Left: 3 AVG: 0.00s  local:3/0/100%/0.0s finished processing [1] [4]
ETA: 0s Left: 2 AVG: 0.00s  local:2/1/100%/3.0s finished processing [2] [5]
ETA: 0s Left: 2 AVG: 0.00s  local:2/1/100%/3.0s finished processing [3] [6]
ETA: 0s Left: 1 AVG: 0.00s  local:1/2/100%/1.5s
ETA: 0s Left: 0 AVG: 0.00s  local:0/3/100%/1.0s
```

### 5.8. Running Parallel Tasks on Remote Machines

We can run our parallel tasks on remote machines using _parallel_ through _ssh_.

Let’s assume we have access to _host1_ and _host2_ using our username and _ssh_ keys that are added to our system. Let’s try it:

```bash
$ parallel -S host1 -S host2 echo ::: running on remote hosts
running
on
remote
hosts
```

**The hosts that will run each command and the order of execution will change randomly with every run.**

## 6. Conclusion[

In this article, we learned how to use the Bash ampersand _&_ operator, _xargs_, and GNU _parallel_ to parallelize our tasks on the command line.
