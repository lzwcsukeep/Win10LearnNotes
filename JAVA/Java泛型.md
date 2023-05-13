## Java泛型一些概念

Java泛型是一个带参数的类或者接口，泛型的参数叫做类型变量(type  variable)，和方法中的参数不同，方法中参数传递的是具体的值，而泛型中的类型变量传递的是类型。

一个非泛型的类可能定义如下：

```java
public class Box {
    private Object object;

    public void set(Object object) { this.object = object; }
    public Object get() { return object; }
}
```

这个类有两个方法和一个属性，由于属性属于Object类型，因此这个类在使用时可以传递任何非基本类型的参数。在编译期也不会有任何的验证来检验这个类是如何被使用的。因此有可能一部分代码存储的Integer 类型，希望取出的也是Integer，但是另一部分代码错误地放进去了String 类型，这就会造成运行时异常。

一个带泛型的Box类：

定义格式： class name<T1, T2, ... ,Tn >{  //code  }

```java
/**
 * Generic version of the Box class.
 * @param <T> the type of the value being boxed
 */
public class Box<T> {
    // T stands for "Type"
    private T t;

    public void set(T t) { this.t = t; }
    public T get() { return t; }
}
```

可以看到Object 出现的地方全部用T代替了。T叫做类型变量，类型变量可以是任何的非基本类型，如类类型，接口类型，数组类型等。

**为什么要使用泛型？**

简而言之，泛型让类型（类，接口）在定义类，接口方法时成为参数，这提供了一种代码重用的方式。

正如大家使用方法中声明的参数一样，类型参数可以让泛型在不同的输入下共用相同的代码。

泛型还有以下好处：

- 将运行时错误提前到编译期

  Java 编译器可以对泛型代码做类型检查，如果产生了类型安全错误可以给出提示，这比在运行时出现错误要好修复的多。

- 免去类型转换

以下代码未使用泛型，需要类型转换

```java
List list = new ArrayList();
list.add("hello") ;
String s = (String) list.get(0) ;
```

使用泛型，无序转换：

```java
List<String> list = new ArrayList();
list.add("hello") ;
String s = list.get(0) ; //no cast
```

- 可以实现通用的算法

 通过使用泛型，程序员可以实现更加类型安全，易读，适用于多种类型的算法。



**名词解释：**

类型变量（type variable）：泛型类型定义时的T,K,V等变量，包括类类型，接口类型，数组类型，甚至是另一个类型变量

参数化后的类型（parametrized type）：指一个泛型类型被使用时传递了一个具体的类型，这个泛型就成为被参数化的类型。

原始类型（raw type）： 当使用一个泛型时，没有传递任何具体类型，那么就叫原始类型。

如：public class Box <T\> { }  使用：Box rawBox = new Box();   //未传递具体类型

因此rawBox 就是泛型Box<T\>的原始类型。注意，非泛型类和原始类型不一样。



原始类型出现在遗留代码中，因为JDK5.0中没有泛型，当使用原始类型时本质上你指定了Object。为了向后兼容，将参数化的类型传递给原始类型是允许的。

如：

```java
Box<String \> stringBox = new Box<>() ;
Box rawBox = stringBox ; //OK
```

但是将原始类型赋值给参数化的类型，会有一个警告：

```java
Box rawBox = new Box(); //rawBox 是Box<T> 的一个原始类型
Box<Integer> intBox = rawBox ;//warning: uncheck conversion
```

当你使用了一个raw type 来调用一个泛型类中定义的泛型方法时，也会有一个警告：

```java
Box<String> stringBox = new Box<>();
Box rawBox = stringBox ;
rawBox.set(8) ;//warning:unchecked invocation to set(T)
```

这些警告显示原始类型绕过了泛型类型检查，将捕获不安全代码从编译期推迟到运行时，因此应避免使用原始类型。

**类型参数命名约定：**

- E - 元素
- K - 键(Key)
- N - 数字
- T - 类型
- V - 值(Value)
- S,U,V 等 - 第二，第三，第四 种类型

这些命名在Java SE API 中经常使用。

