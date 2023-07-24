不能，C++析构函数不能带参数，也不返回任何东西。

> It never takes any parameters, and it never returns anything.
> 来自isocpp/wiki/faq/dtors

---

> **Can I overload the destructor for my class?**  
> No.
> You can have only one destructor for a class Fred. It’s always called Fred::~Fred(). It never takes any parameters, and it never returns anything.

You can’t pass parameters to the destructor anyway, since you never explicitly call a destructor (well, almost never).

[isocpp/wiki/faq/dtors](https://isocpp.org/wiki/faq/dtorshttps://isocpp.org/wiki/faq/dtors)
