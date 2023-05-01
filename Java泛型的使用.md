## Java泛型的使用

**1.泛型类**
为什么要定义泛型类？
某些操作或属性对多种数据类型通用，则可以定义带泛型的类。在定义时表示通用性
在使用时确定数据类型。可以减少重复代码编写。
所以一般定义泛型了之后的类，在使用时指定类型，那么出现泛型的地方都用指定类型代替。

**怎么定义泛型类？**     

和普通类定义基本相同，不过在类名后加一个通用类型标识。
格式：修饰符 class 类名<泛型>{ }
位置：正常类定义在哪，泛型类也可以
使用：在其他类中创建对象时，指定数据类型，不指定默认Object类型
例子：

一个带泛型的类

```java
package Test_Lzw;

public class GenericClass<E> {
    E name ;

    public void setName(E name){
        this.name = name ;
    }
    public E getName(){
        return name ;
    }
}
```
在另一个类中使用泛型类创建对象
```java
package Test_Lzw;

public class GenericDemo {
    public static void main(String[] args) {
        GenericClass ge = new GenericClass();
        ge.setName("字符串");
        Object name = ge.getName();
        System.out.println(name);

        GenericClass<Integer> demo2 = new GenericClass<>(); //创建对象时指定类型
        demo2.setName(2);
        Integer name1 = demo2.getName();
        System.out.println(name1);
        GenericClass<Double> d3 = new GenericClass<>();
        d3.setName(8.8);
        Double name2 = d3.getName();
        System.out.println(name2);


    }
}
```
输出如下：
```text
    字符串
    2
    8.8
```

**2.泛型接口**

泛型接口定义和类差不多，就是在原有接口定义格式的接口名后多了一个"<类型变量>"
定义好之后类型变量可以在整个接口中使用

格式： 修饰符 interface 接口名<类型变量>{ }

泛型接口的使用

接口需要实现类才能使用，有以下两种情况

1.定义实现类时指定接口的泛型类型，该实现类可以当成普通类使用
2.定义实现类时不指定确定的类型而是使用接口同样的泛型。此时实现类仍然是泛型类

例子：

GenericInterface,GenericInterfaceImpl,GenericInterfaceDemo 是第一种使用
GenericInterface,GenericInterfaceImpl2,GenericInterfaceDemo2 是第二种使用



**1) 定义实现类时指定具体的类型：**

接口：

```java
package Test_Lzw;

public interface GenericInterface<E> {
    public abstract void method1(E e);
}

```

实现类：

```java
package Test_Lzw;

public class GenericInterfaceImpl implements GenericInterface<String> {

    @Override
    public void method1(String s) {
        System.out.println("实现类指定了类型");
        System.out.println(s);
        System.out.println(s);


    }
}
```

使用：

```java
package Test_Lzw;

public class GenericInterfaceDemo {
    public static void main(String[] args) {
        GenericInterfaceImpl gil = new GenericInterfaceImpl();
        gil.method1("");
    }
}

```

输出：

```te

实现类指定了类型
```

**2）定义实现类时不指定确定的类型而是使用接口同样的泛型**

实现类和接口使用一样的泛型类型

```java
package Test_Lzw;

public class GenericInterfaceImpl2<E> implements GenericInterface<E> {
    @Override
    public void method1(E e) {
        System.out.println("实现类使用和接口一样的泛型");
        System.out.println(e);
    }

}

```

使用时相当于一个泛型类

```java
package Test_Lzw;

public class GenericInterfaceDemo2 {
    public static void main(String[] args) {
        GenericInterfaceImpl2<Integer> gil2 = new GenericInterfaceImpl2<>();
        gil2.method1(1);

        GenericInterfaceImpl2<Double> gil3 = new GenericInterfaceImpl2();
        gil3.method1(4.0);
        GenericInterfaceImpl2<String> gil4 = new GenericInterfaceImpl2();
        gil4.method1("77");
    }

}
```

输出：

```java
实现类使用和接口一样的泛型
1
实现类使用和接口一样的泛型
4.0
实现类使用和接口一样的泛型
77
```

**3.泛型方法**

格式： 修饰符 <类型变量> 返回值类型 方法名（参数）{ }
      在方法返回值之前写一个泛型标志
位置：可以定义在一个非泛型类之中
使用：先创建一个方法所在类的对象，然后通过对象调用该方法
     如果是静态方法，则直接通过类名.方法名调用
例子：
一个定义了泛型方法的类GenericMethod

```java
package Test_Lzw;

public class GenericMethod {

    public <T> void method1(T t){
        System.out.println(t);
    }
}
```
在另一个类中使用泛型方法
```java
package Test_Lzw;

public class Demo03GenericMethod {
    public static void main(String[] args) {
        GenericMethod gm = new GenericMethod() ; //创建对象
        gm.method1("nice");                //调用方法，可以传递任意类型参数
        gm.method1(2);
        gm.method1(true);

    }

}
```

注：泛型方法不需要依赖泛型类存在