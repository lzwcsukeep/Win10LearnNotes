### 1.无名参数激活函数重载

1. 什么是无名参数?

无名参数(unnamed parameters)就是在函数定义或者声明时只给参数类型而不给参数名。

定义时既然是无名参数，就表明在函数体内不需要使用这个参数。

问：既然在函数体内无需使用，那么为什么还要占用一个参数位置呢？

答：可以用来激活函数重载

看下面代码：

```cpp
#include <iostream>

void printMessage(int){
    std::cout << "无名参数,int" << std::endl ;
}

void printMessage(double){
    std::cout << "无名参数,double" << std::endl ;
}
int main()
{
    printMessage(int(5)) ;
    printMessage(double()) ;
    return 0 ;
}
```

上述代码`printMessage`的参数完全没有在函数体内用到，但是我们可以根据传递的参数不同类型，调用不同版本的`printMessage` ,这里的无名参数，仅仅用来激活不同版本。这个功能在STL中针对不同类型迭代器(前向或随机访问等)，设计同一功能函数的不同版本时十分有用。
