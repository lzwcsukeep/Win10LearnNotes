C++ 的类中可以含有静态成员，使用`static`关键字修饰。

C++类的静态成员是和类相关的，所有的这个类的对象共享同一个静态成员。

静态方法成员中没有this 指针。

静态方法不能访问非静态成员。

> Classes can also have static member functions. These represent the same: members of a class that are common to all object of that class, acting exactly as non-member functions but being accessed like members of the class. Because they are like non-member functions, they cannot access non-static members of the class (neither member variables nor member functions). They neither can use the keyword `this`.

静态成员变量不能直接在类内部初始化，而是需要在类的外部某处。

> In fact, static members have the same properties as non-member variables but they enjoy class scope. For that reason, and to avoid them to be declared several times, they cannot be initialized directly in the class, but need to be initialized somewhere outside it.


