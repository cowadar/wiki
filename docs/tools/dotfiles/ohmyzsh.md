---
title: Oh My ZSH
---

<p align="center"><img src="https://ohmyzsh.s3.amazonaws.com/omz-ansi-github.png" alt="Oh My Zsh"></p>

## Zsh?

**Oh My Zsh** is a framework for [Zsh](https://www.zsh.org), the Z shell.

- In order for **Oh My Zsh** to work, **Zsh must be installed**.
  - Please run `zsh --version` to confirm.
  - Expected result: `zsh 5.0.8` or more recent
- Additionally, Zsh should be set as your default shell.
  - Please run `echo $SHELL` from a new terminal to confirm.
  - Expected result: `/usr/bin/zsh` or similar

## Install and set up zsh as default

If necessary, follow these steps to install Zsh:

1. There are two main ways to install Zsh:

   - With the package manager of your choice, _e.g._ `sudo apt install zsh` (see [below for more examples](#how-to-install-zsh-on-many-platforms))
   - From [source](https://zsh.sourceforge.io/Arc/source.html), following [the instructions from the Zsh FAQ](https://zsh.sourceforge.io/FAQ/zshfaq01.html#l7).

2. Verify installation by running `zsh --version`. Expected result: `zsh 5.0.8` or more recent.

3. Make it your default shell: `chsh -s $(which zsh)` or use `sudo lchsh $USER` if you are on Fedora.

   - Note that this will not work if Zsh is not in your authorized shells list (`/etc/shells`)
    or if you don't have permission to use `chsh`. If that's the case [you'll need to use a different procedure](https://www.google.com/search?q=zsh+default+without+chsh).
    - If you use `lchsh` you need to type `/bin/zsh` to make it your default shell.

4. Log out and log back in again to use your new default shell.

5. Test that it worked with `echo $SHELL`. Expected result: `/bin/zsh` or similar.

6. Test with `$SHELL --version`. Expected result: 'zsh 5.8' or similar

## How to install zsh on many platforms

### macOS

**Try `zsh --version` before installing it from Homebrew. Preferably newer than or equal to `5.0.8`.**

```sh
brew install zsh
```

To set zsh as your default shell, execute the following assuming a default install of Homebrew

- Recent macOS versions:

  For m1 macs:

  ```sh
  chsh -s /opt/homebrew/bin/zsh
  ```

  For intel macs:

  ```sh
  chsh -s /usr/local/bin/zsh
  ```

- macOS **High Sierra** and older:

  ```sh
  chsh -s /bin/zsh
  ```

Assuming you have [Homebrew](https://brew.sh/) installed. If not, most versions of
**macOS** ship zsh by default, but it's normally an older version.  Alternatively, you may
also use [MacPorts](https://www.macports.org/)

```sh
sudo port install zsh zsh-completions
```

### Ubuntu, Debian & derivatives (Windows 10 WSL | Native Linux kernel with Windows 10 build 1903)

```sh
apt install zsh
```

If you don't have `apt`, the recommended package manager for end users
[[1]](https://askubuntu.com/a/446484)
[[2]](https://askubuntu.com/a/775264)
[[3]](https://help.ubuntu.com/lts/serverguide/apt.html)
[[4]](https://www.howtogeek.com/234583/simplify-command-line-package-management-with-apt-instead-of-apt-get/)
, you can try `apt-get` or `aptitude`.

[Other distributions that apply](https://en.wikipedia.org/wiki/List_of_Linux_distributions#Debian-based) include:
Linux Mint, elementary OS, Zorin OS, Raspbian, MX Linux, Deepin.

### OpenSUSE

```sh
zypper install zsh
```

### Arch Linux or Manjaro

```sh
pacman -S zsh
```

### Void Linux

```sh
xbps-install zsh
```

### Fedora

```sh
dnf install zsh
```

### OpenBSD

To install the package:

```sh
pkg_add zsh
```

### FreeBSD

To install the package:

```sh
pkg install zsh
```

To install the port:

```sh
cd /usr/ports/shells/zsh/ && make install clean
```

To reduce memory usage, optionally enable zsh-mem options with
![installation screen to enable zsh-mem](https://i.imgur.com/l4id6Ek.png)

```sh
make config
```

before running "make install".

### Centos/RHEL

```sh
sudo yum update && sudo yum -y install zsh
```

### Cygwin

Install the zsh package using the installer. Unfortunately Cygwin doesn't have a standard command line interface. You could, however, setup [apt-cyg](https://github.com/kou1okada/apt-cyg) and install zsh as follows:

```sh
apt-cyg install zsh
```

The easiest way to change the default shell is to set your SHELL user environment variable. Search for "Edit Environment variables for your account" to bring up the environment variables window, create a new variable named "SHELL" and give it the value "/usr/bin/zsh/".

*Alternatively:*
Open Cygwin (in BASH) then type:

```sh
sudo nano ~/.bashrc
```

Once the .bashrc file is open, add this line to the very top:

```sh
exec zsh
```

Close and save the file.
Close and reopen Cygwin.
It will execute the command every time you load the terminal and run your zsh shell.

### Solus

```sh
eopkg it zsh
```

### Funtoo/Gentoo

```sh
emerge app-shells/zsh
```

### Alpine Linux

```sh
apk add zsh
```

### MSYS2

```sh
pacman -S zsh
```

### Termux (Android)

Termux is an terminal emulator for Android but has modern feature like Debian and Ubuntu (Termux has Bash shell and Busybox GNU-like programs). For the package manager, Termux using an Debian/Ubuntu package manager, APT.
To install the package, run this command:

```sh
pkg install zsh
```

The command looks like FreeBSD package manager (`pkg`). Or you can run this command:

```sh
apt update && apt upgrade
apt install zsh
```

To set zsh as your default shell, run this command:

```sh
chsh -s zsh
```

## Oh My Zsh

Oh My Zsh is an open source, community-driven framework for managing your [zsh](https://www.zsh.org/) configuration.

- You'll need `zsh` to install **Oh My Zsh**. Run `zsh --version` to check if you have it:

  ```console
  $ zsh --version
  zsh 5.8 (x86_64-ubuntu-linux-gnu)
  ```

- If you see `command not found` you don't have zsh installed. See **Installing Zsh** for instructions.
- Once you have zsh, you can install Oh My Zsh by simply running one of these commands:

  | Method    | Command                                                                                           |
  | :-------- | :------------------------------------------------------------------------------------------------ |
  | **curl**  | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
  | **wget**  | `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`   |
  | **fetch** | `sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |

  **NOTE: the installer will rename an existing `.zshrc` file to `.zshrc.pre-oh-my-zsh`.**

## Cheatsheet

Quick reference:

- Oh My Zsh's Command-Line Interface (CLI): `omz`. Run `omz --help` for available commands.
- To **update** Oh My Zsh: `omz update`.
- To **uninstall** it: `uninstall_oh_my_zsh`.
- To apply changes made to `.zshrc`: `omz reload` (this just runs `exec zsh`).
  [**Do NOT run `source ~/.zshrc`**](https://github.com/ohmyzsh/ohmyzsh/wiki/FAQ#how-do-i-reload-the-zshrc-file).

## Commands

| Command     | Description                                                                                                                                                                                                                                                                                                         |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `alias`     | List all aliases                                                                                                                                                                                                                                                                                                    |
| `mkcd`      | Create a new directory and change to it, will create intermediate directories as required.                                                                                                                                                                                                                          |
| `take`      | Like `mkcd`, but also knows how to handle remote URLs. When given an argument that looks like a URL (something that ends in `.git` or `.tar.(gz\|bz2\|xz)`), download the remote resource and extract it (if necessary) into the current directory. Then change to the newly extracted/downloaded/cloned directory. |
| `zsh_stats` | Get a list of the top 20 commands and how many times they have been run.                                                                                                                                                                                                                                            |

## Directory

| Alias      | Command                                    |
| :--------- | :----------------------------------------- |
| `md`       | `mkdir -p`                                 |
| `rd`       | `rmdir`                                    |
| `cd` / `~` | `cd` to your home directory                |
| `..`       | `cd ..`                                    |
| `...`      | `cd ../..`                                 |
| `....`     | `cd ../../..`                              |
| `.....`    | `cd ../../../..`                           |
| `/`        | `cd /`                                     |
| `d`        | `dirs -v` (lists last visited directories) |
| `cd +n`    | Switch to directory number `n`             |
| `-`        | `cd` to last visited directory             |
| `1`        | `cd -1`                                    |
| `2`        | `cd -2`                                    |
| `3`        | `cd -3`                                    |
| `4`        | `cd -4`                                    |
| `5`        | `cd -5`                                    |
| `6`        | `cd -6`                                    |
| `7`        | `cd -7`                                    |
| `8`        | `cd -8`                                    |
| `9`        | `cd -9`                                    |