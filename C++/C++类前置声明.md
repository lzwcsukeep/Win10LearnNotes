**C++ 类的前置声明**

类似于函数的声明和定义，C++里类的声明和定义也是可以分开的。我们可以先声明而暂时不定义它，这种声明就称为类的前置声明，forward declaration。

```cpp
class Screen;
```

这个前置声明在代码里引入了名字Screen，并指示Screen是一个类类型。

对于类类型Screen来说，在它声明之后定义之前是一个不完全类型，所谓不完全类型就是我们知道Screen是一个类类型，但是我们不知道它到底包含了哪些成员。

**不完全类型只能在非常有限的情况下使用：**

1. 只能定义指向这种不完全类型的指针或引用

2. 只能声明（但是不可以定义）以不完全类型作为参数或者返回类型的函数

以下代码是一个错误例子，在类Link_Screen里不能使用Screen去创建对象，只能去定义Screen类的指针或引用:

```cpp
class Screen; // Screen的前置声明

class Link_Screen
{
public:
    Screen window; // 错误，创建对象时该类必须已经定义过
    Link_Screen* prev;
    Link_Screen* next;
};


```

以下代码是正确例子，

```cpp
class Screen; // Screen的前置声明
class Link_Screen
{
public:
    Screen* window; // 正确，只能创建Screen类的指针或引用

    Link_Screen* prev;
    Link_Screen* next;
};
```

类前置声明的好处和坏处？

好处：

1. 节约编译时间

2. 解决两个类相互依赖的问题。
   
   

具体的知识需要之后去补。可以先详细的看看代码结构，然后在去补一补这个类的前置声明知识点。




