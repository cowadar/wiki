# test

??? note

    ===  "c++"
        ```
        #include <iostream>

        int main(void) {
        std::cout << "Hello world!" << std::endl;
        return 0;
        }
        ```
    === "Unordered List"
        ```
        #include <iostream>

        int main(void) {
        std::cout << "Hello world!" << std::endl;
        return 0;
        }
        ```


??? note

    !!! example

        === "Unordered List"

            markdown
            * Sed sagittis eleifend rutrum
            * Donec vitae suscipit est
            * Nulla tempor lobortis orci
            

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
    
