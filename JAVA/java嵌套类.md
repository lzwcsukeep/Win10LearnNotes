#### Java 嵌套类(Nested Class)

为什么要叫嵌套类而不是内部类？

因为网上不区分这两种叫法是有问题的，嵌套类和内部类是有区别的不能混淆，嵌套类的概念包括了内部类。只有非静态的嵌套类才叫做内部类。

> Java官方： 
>
> Nested classes are divided into two categories: non-static and static. Non-static nested classes are called *inner classes*. Nested classes that are declared `static` are called *static nested classes*.



**嵌套类**

Java语言允许在一个类的内部定义另一个类，这样的类就叫嵌套类。也就是说，只要是定义在一个类的内部的类就统称为嵌套类。

嵌套类分两种：Non-static（非静态的） 和 static（静态的）

- Non-static nested class（非静态嵌套类）也叫inner classes（内部类）
  - 成员内部类(Member inner class)
  - 匿名内部类(Anonymous inner class)
  - 局部内部类(Local inner class)
- static nested class,静态嵌套类，用static 关键字修饰的嵌套类



一个嵌套类是其封装类的一个成员。非静态的嵌套类（内部类）可以访问外部类的所有成员，包括被private 修饰的。作为外部类的一个成员，嵌套类可以被`private` ,`public`，`protected` 或者包限定符修饰。



嵌套类的好处：

1. 将只会在一处地方使用的类从逻辑上组织起来

> 如果一个类只对另一个类有用，那么将其嵌入该类并将两者保持在一起是合乎逻辑的。嵌套这样的“助手类”使它们的包更加精简。

2. 增加封装性
3. 增加可读性和可维护性



关于静态嵌套类的描述：

<https://www.javatpoint.com/static-nested-class>