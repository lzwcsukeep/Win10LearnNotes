### 使用typedef 定义一个函数指针类型。

```c
// C Program for how to create a Typedef for a Function 
// Pointer 
#include <stdio.h> 

typedef int (*Operation)(int, int); 
int add(int a, int b) { return a + b; } 

// Function to the subtract two integers 
int subtracts(int a, int b) { return a - b; } 

// Driver Code 
int main() 
{ 
    // Declare function pointers using typedef 
    Operation operationAdd = add; 
    Operation operationSubtract = subtracts; 
    printf("Addition result: %d\n", operationAdd(20, 9)); 
    printf("Subtraction result: %d\n", 
        operationSubtract(20, 9)); 
    return 0; 
}
```

仔细看上述代码，定义了一个新类型。函数指针类型。然后可以用该类型声明变量。

`typedef int (*Operation)(int, int);`

`Operation operationAdd = add; `

`Operation operationSubtract = subtracts;`

### 如何调用上述函数指针：

有两种方法。如

对于下面声明的函数指针

```c
void (*pf)(int foo, int bar);
```

这两种调用方式等价：

```c
pf(1, 0);
(*pf)(1, 0);
```

有了上述基础，就可以理解回调函数了。
