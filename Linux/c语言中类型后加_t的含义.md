C 语言中常看到size_t, off_t, mode_t 这样后缀为_t的类型。这里_t的含义是什么呢？

_t 表示data type。

这里的_t对编译器来说没有任何特别，只是人为的惯例，一般表示这是一个使用了typedef 的类型。

From chatGPT:

In C programming, the `_t` suffix conventionally indicates that a type is a typedef. `mode_t` is a type defined using a typedef in the C standard library.

The `_t` suffix stands for "type", so `mode_t` represents a type that is used to represent file modes or permissions in Unix-like operating systems.

For example, in the `<sys/stat.h>` header file, you might find the typedef declaration for `mode_t`:

```c
typedef unsigned int mode_t;
```

This declaration defines `mode_t` as an alias for the `unsigned int` data type. It's essentially a way to create a more descriptive name for a particular type, enhancing code readability and maintainability.
