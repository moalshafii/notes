# Memory Regions Notes for C/C++ and JS

This note explains memory regions and gives examples for **C/C++** and **JavaScript**.

---

## C / C++ Example

```c
#include <stdio.h>
#include <stdlib.h>

int global_init = 42;           // DATA (.data)
int global_uninit;               // BSS (.bss)
const int global_const = 100;    // RODATA (read-only)
static int static_init = 5;      // DATA (.data)
static int static_uninit;        // BSS (.bss)
const char *msg = "Hello";      // RODATA

void func() {
    int local_var = 10;          // STACK
    const int local_const = 20;  // STACK
    int *heap_var = (int *)malloc(sizeof(int)); // HEAP
    *heap_var = 99;

    printf("Addresses:\n");
    printf("global_init: %p\n", (void*)&global_init);
    printf("global_uninit: %p\n", (void*)&global_uninit);
    printf("global_const: %p\n", (void*)&global_const);
    printf("static_init: %p\n", (void*)&static_init);
    printf("static_uninit: %p\n", (void*)&static_uninit);
    printf("msg: %p\n", (void*)msg);
    printf("local_var: %p\n", (void*)&local_var);
    printf("local_const: %p\n", (void*)&local_const);
    printf("heap_var: %p\n", (void*)heap_var);

    free(heap_var);
}

int main() {
    func();
    return 0;
}
```

### Memory Regions Table (C/C++)

| Variable        | Region | Example                      |
| --------------- | ------ | ---------------------------- |
| `global_init`   | DATA   | Initialized global           |
| `global_uninit` | BSS    | Uninitialized global         |
| `global_const`  | RODATA | Read-only constant           |
| `static_init`   | DATA   | Initialized static           |
| `static_uninit` | BSS    | Uninitialized static         |
| `msg`           | RODATA | String literal               |
| `local_var`     | STACK  | Local variable               |
| `local_const`   | STACK  | Local const                  |
| `heap_var`      | HEAP   | malloc dynamically allocated |

---

## JavaScript Example

```js
// Global constant → in JS engine's memory (RODATA-like)
const globalConst = 100;

// Global variable → heap (managed by GC)
let globalVar = { name: 'MAS' };

function func() {
    // Local variables → stack (primitives), heap (objects)
    let localVar = 10;           // primitive → stack
    const localConst = 20;       // primitive → stack
    let localObj = { x: 5 };     // object → heap

    console.log(globalConst, globalVar, localVar, localConst, localObj);
}

func();
```

### Memory Regions Table (JS)

| Variable      | Region           | Notes                            |
| ------------- | ---------------- | -------------------------------- |
| `globalConst` | Read-only / Heap | Constant stored in engine memory |
| `globalVar`   | Heap             | Object allocated dynamically     |
| `localVar`    | Stack            | Primitive stored on call stack   |
| `localConst`  | Stack            | Primitive stored on call stack   |
| `localObj`    | Heap             | Object allocated dynamically     |

---

### Notes:

* **C/C++**: Stack + Heap + Data + BSS + RODATA + Text sections explicitly exist.
* **JS**: Stack + Heap exist conceptually, GC manages heap; constants and strings may be in engine-managed memory.
* **Conceptually**, all programs have **code, constants, globals/statics, heap, and stack**. Memory layout differs in implementation.
