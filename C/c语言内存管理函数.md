### C 动态内存管理函数

如下图，主要介绍前四个`malloc` ,`calloc` ,`realloc` ,`free`

![](C:\Users\lzw\AppData\Roaming\marktext\images\2022-08-08-15-21-38-image.png)

#### malloc

| Defined in header `<stdlib.h>`                                              |     |
| --------------------------------------------------------------------------- | --- |
| void* malloc( [size_t](http://en.cppreference.com/w/c/types/size_t) size ); |     |

Allocates `size` bytes of uninitialized storage.

If allocation succeeds, returns a pointer that is suitably aligned for any object type with [fundamental alignment](https://en.cppreference.com/w/c/language/object#Alignment "c/language/object").

If `size` is zero, the behavior of `malloc` is implementation-defined. For example, a null pointer may be returned. Alternatively, a non-null pointer may be returned; but such a pointer should not be [dereferenced](https://en.cppreference.com/w/c/language/operator_member_access "c/language/operator member access"), and should be passed to [free](https://en.cppreference.com/w/c/memory/free "c/memory/free") to avoid memory leaks.

**参数**

size - 需要分配的字节数

**返回值**

如果成功，返回新分配的内存的起始地址。为了避免内存泄漏，分配的内存需要用`free` 或者`realloc` 函数回收。

```c
#include <stdio.h>   
#include <stdlib.h> 

int main(void) 
{
    int *p1 = malloc(4*sizeof(int));  // allocates enough for an array of 4 int
    int *p2 = malloc(sizeof(int[4])); // same, naming the type directly
    int *p3 = malloc(4*sizeof *p3);   // same, without repeating the type name

    if(p1) {
        for(int n=0; n<4; ++n) // populate the array
            p1[n] = n*n;
        for(int n=0; n<4; ++n) // print it back out
            printf("p1[%d] == %d\n", n, p1[n]);
    }

    free(p1);
    free(p2);
    free(p3);
}
```

输出

```c
p1[0] == 0
p1[1] == 1
p1[2] == 4
p1[3] == 9
```

#### calloc

| Defined in header `<stdlib.h>`                                                                                                         |     |     |
| -------------------------------------------------------------------------------------------------------------------------------------- | --- | --- |
| void* calloc( [size_t](http://en.cppreference.com/w/c/types/size_t) num, [size_t](http://en.cppreference.com/w/c/types/size_t) size ); |     |     |

Allocates memory for an array of `num` objects of `size` and initializes all bytes in the allocated storage to zero.

If allocation succeeds, returns a pointer to the lowest (first) byte in the allocated memory block that is suitably aligned for any object type.

**参数**

num - number of objects

size - size of object

**返回值**

如果成功，返回新分配的内存的起始地址。为了避免内存泄漏，分配的内存需要用`free` 或者`realloc` 函数回收。失败返回一个空指针。

### realloc

扩展或缩小之前分配过的内存空间，`realloc` 作用的内存空间必须提前被`malloc` 或者 `calloc` 分配过，并且没有使用`free` 释放。否则，行为将是未定义。

分配行为可能是在原有空间上进行扩大或缩小，也可能是开辟一段新内存，然后将旧空间的元素复制过去。并`free` 旧空间。

| Defined in header `<stdlib.h>`                                                              |     |     |
| ------------------------------------------------------------------------------------------- | --- | --- |
| void *realloc( void *ptr, [size_t](http://en.cppreference.com/w/c/types/size_t) new_size ); |     |     |

参数：

ptr - pointer to the memory area to be reallocated.

new_size - new size of the array in bytes.

返回值：

如果成功，返回新内存的首地址。为了避免内存泄漏，重分配的内存需要用`free` 回收。旧的指针失效，并且对旧指针的解引用操作是未定义行为。

如果失败，返回空指针，旧指针还有效。

#### free

| Defined in header `<stdlib.h>` |     |     |
| ------------------------------ | --- | --- |
| void free( void* ptr );        |     |     |

Deallocates the space previously allocated by [malloc()](https://en.cppreference.com/w/c/memory/malloc "c/memory/malloc"), [calloc()](https://en.cppreference.com/w/c/memory/calloc "c/memory/calloc"), aligned_alloc(), (since C11) or [realloc()](https://en.cppreference.com/w/c/memory/realloc "c/memory/realloc").

If `ptr` is a null pointer, the function does nothing.

参数：

ptr - pointer to the memory to deallocate

返回值：

None

```c
#include <stdlib.h>

int main(void)
{
    int *p1 = malloc(10*sizeof *p1);
    free(p1); // every allocated pointer must be freed

    int *p2 = calloc(10, sizeof *p2);
    int *p3 = realloc(p2, 1000*sizeof *p3);
    if(p3) // p3 not null means p2 was freed by realloc
       free(p3);
    else // p3 null means p2 was not freed
       free(p2);
}
```

总结：

涉及c语言的动态内存管理的函数主要有四个，malloc ,calloc,realloc,free .

malloc只有一个参数size .表示分配的内存字节数，未初始化。

calloc 分配一个数组所需的空间，有两个参数num 表示数组元素的个数，size 表示元素的大小。并且空间初始化为0。如果有内存对齐，最终分配的空间大小可能并不等于num \* size。

realloc 对已经分配过的内存进行重新分配调整大小。参数有两个，旧空间的指针和新空间所占的字节数。

free 对分配的空间进行回收。参数是指向待回收空间起始位置的指针。

四个函数的详细信息可在cppreference 中查看。

[Dynamic memory management - cppreference.com](https://en.cppreference.com/w/c/memory)
