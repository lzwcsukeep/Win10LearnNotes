简略版：

需要用到的API:

va_list 类型的变量ap，该类型在stdarg.h中定义。

宏va_start()  //初始化参数列表

宏va_arg()   // 实际访问可变参数，并使得va_list 变量指向下一个可变参数

宏va_end()  // 结束可变参数列表的使用

---

详细版：

C语言可以定义可变参数函数，可变参数函数的含义就是函数可以接受可变数量的参数，而不是固定数量的参数。

可变参数函数的形式：

```c
int func_name(int arg1, ...);
```

其中，省略号 ... 表示可变参数列表。

自定义可变参数函数需要用到的变量或宏：

变量：

一个**va_list**类型的变量ap,该类型是在 stdarg.h 头文件中定义的。

宏：

**va_start(ap,last_arg)** :初始化可变参数列表。`ap` 是一个 `va_list` 类型的变量，`last_arg` 是最后一个固定参数的名称（也就是可变参数列表之前的参数）。该宏将 `ap` 指向可变参数列表中的第一个参数。

**va_arg(ap,type)**: 获取可变参数列表中的下一个参数。`ap` 是一个 `va_list` 类型的变量，`type` 是下一个参数的类型。该宏返回类型为 `type` 的值，并将 `ap` 指向下一个参数。

**va_end(ap)** : 结束可变参数列表的访问。`ap` 是一个 `va_list` 类型的变量。该宏将 `ap` 置为 `NULL`。

实际步骤：

为了使用这个功能，您需要使用 **stdarg.h** 头文件，该文件提供了实现可变参数功能的函数和宏。具体步骤如下：

- 定义一个函数，最后一个参数为省略号，省略号前面可以设置自定义参数。
- 在函数定义中创建一个 **va_list** 类型变量，该类型是在 stdarg.h 头文件中定义的。
- 使用 **int** 参数和 **va_start()** 宏来初始化 **va_list** 变量为一个参数列表。宏 **va_start()** 是在 stdarg.h 头文件中定义的。
- 使用 **va_arg()** 宏和 **va_list** 变量来访问参数列表中的每个项。
- 使用宏 **va_end()** 来清理赋予 **va_list** 变量的内存。

按照上面的步骤自定义一个可变参数函数并使用它。

```cpp
#include <stdio.h>
#include <stdarg.h>

double average(int num,...)
{

    va_list valist;
    double sum = 0.0;
    int i;

    /* 为 num 个参数初始化 valist */
    va_start(valist, num);

    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
       sum += va_arg(valist, int);
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);

    return sum/num;
}

int main()
{
   printf("Average of 2, 3, 4, 5 = %f\n", average(4, 2,3,4,5));
   printf("Average of 5, 10, 15 = %f\n", average(3, 5,10,15));
}
```

在上面的例子中，average() 函数接受一个整数 num 和任意数量的整数参数。函数内部使用 va_list 类型的变量 va_list 来访问可变参数列表。在循环中，每次使用 va_arg() 宏获取下一个整数参数，并输出。最后，在函数结束时使用 va_end() 宏结束可变参数列表的访问。

当上面的代码被编译和执行时，它会产生下列结果。应该指出的是，函数 **average()** 被调用两次，每次第一个参数都是表示被传的可变参数的总数。省略号被用来传递可变数量的参数。

output:

```c
Average of 2, 3, 4, 5 = 3.500000
Average of 5, 10, 15 = 10.000000
```

Refer:

[C 可变参数 | 菜鸟教程](https://www.runoob.com/cprogramming/c-variable-arguments.html)
