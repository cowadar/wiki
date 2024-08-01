# test



<details>
<summary>docker-compose.yaml ZO ZAL HET ZIJN</summary>
```python
def greet(name):
    print("Hello,", name + "!")
test
test
test
bla
``` 
</details>

---
**NOTE**

It works with almost all markdown flavours (the below blank line matters).

---


> [!NOTE]  
> Highlights information that users should take into account, even when skimming.
> test

> **Notitie:** 
>
> Hier is een codeblok binnen een noot:
>
> ```python
> print("Hello, world!")
> ```


<details>
<summary>Codeblok</summary>
```python
def greet(name):
    print("Hello,", name + "!")
```

</details>







??? warning


    test
    test
    test
    test





??? note
    <details>
    <summary>docker-compose.yaml</summary>
    ```python
    def greet(name):
        print("Hello,", name + "!")
    ``` 
    </details>

<details>
  <summary>Tab 1</summary>
  </details>

<details>
  <summary>Tab 2</summary>
  </details>



=== "Unordered List"
<details>
<summary>docker-compose.yaml</summary>
```python
def greet(name):
    print("Hello,", name + "!")
```
</details>

    

=== "Ordered List"

    ``` markdown
    1. Sed sagittis eleifend rutrum
    2. Donec vitae suscipit est
    3. Nulla tempor lobortis orci
    ```

<style>
  .code-container {
    display: flex;
  }

  .code-block {
    flex: 1;
    border: 1px solid #ccc;
    padding: 10px;
    background-color: #f5f5f5;
  }
</style>

<div class="code-container">
  <div class="code-block">
    
    test
    test
    </div>
  <div class="code-block">
    </div>
</div>


??? note
    === "Unordered List"

        ``` yaml
        api:
        dashboard: true
        debug: true

        entryPoints:
        http:
            address: ":80"
            forwardedHeaders:
            trustedIPs: &trustedIps
                # Start of Clouflare public IP list for HTTP requests, remove this if you don't use it
                - 173.245.48.0/20
                - 103.21.244.0/22
        ```     

    === "Ordered List"

        ``` markdown
        1. Sed sagittis eleifend rutrum
        2. Donec vitae suscipit est
        3. Nulla tempor lobortis orci
        ```

=== "C"

    ``` c
    #include <stdio.h>

    int main(void) {
      printf("Hello world!\n");
      return 0;
    }
    ```

=== "C++"

    ``` c++
    #include <iostream>

    int main(void) {
      std::cout << "Hello world!" << std::endl;
      return 0;
    }
    ```


``` c++
#include <iostream>

int main(void) {
    std::cout << "Hello world!" << std::endl;
    return 0;
}
```

## Tabs 

```curl
$ curl -O wget http://example.com/pk.zip
```

```wget
$ wget http://example.com/pk.zip
```

## Single block

```
$ ls -lisa
```


    py title="bubble_sort.py"
    def bubble_sort(items):
        for i in range(len(items)):
            for j in range(len(items) - 1 - i):
                if items[j] > items[j + 1]:
                    items[j], items[j + 1] = items[j + 1], items[j]
    
