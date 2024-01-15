```cpp
#include <iostream>

#ifdef MMM
    // This code will be included if MMM is defined
    #define MESSAGE "Macro MMM is defined!"
#else
    // This code will be included if MMM is not defined
    #define MESSAGE "Macro MMM is not defined."
#endif

int main() {
    std::cout << MESSAGE << std::endl;
    return 0;
}
```

上面代码可以根据宏 `MMM` 是否被定义而有不同的行为。

宏MMM 可以使用g++ 的 `-D`选型定义。

### #ifdef 是什么？

`#ifdef` is a preprocessor directive in C and C++. It stands for "if defined" and is used for **conditional compilation**. Conditional compilation allows parts of the code to be included or excluded during the compilation process based on certain conditions.

The basic syntax of `#ifdef` is:

```cpp
#ifdef identifier
    // Code to include if the identifier is defined
#endif
```

Here, `identifier` is usually a macro or a symbol. If `identifier` is defined using a `#define` directive earlier in the code or through compiler options, then the code between `#ifdef` and `#endif` will be included in the compilation. If `identifier` is not defined, the code between `#ifdef` and `#endif` will be excluded.

注： 条件编译指令的妙用，可以决定哪些代码进入编译阶段，从而使程序有不同的行为或达到跨平台的目的。
