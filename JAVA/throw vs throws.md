## Java中throw 和 throws 关键字的不同

throw 和 throws 都是 异常处理中的概念，然而 throw 关键字用来在一个方法或一个代码块中显式的抛出一个异常，throws 关键字用在方法签名中。

下面是关键字throw 和关键字 throws 的一些区别：

| Sr. no. | Basis of Differences                                         | throw                                                        |                            throws                            |
| :------ | :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------: |
| 1.      | 定义                                                         | Java关键字throw 用来在函数或者代码块中显式抛出一个异常       | Java throws 关键字用在方法签名上来声明一个异常，这个方法的代码在执行时有可能会抛出这个异常 |
| 2.      | 使用throw关键字的异常类型，我们只能传播非检查型异常，即，仅使用throw不能传播检查型异常 | 使用throws关键字，我们可以声明检查型和非检查型的异常。但是，throws关键字只能用于传播检查型的异常 |                                                              |
| 3.      | 语法                                                         | throw 关键字后紧跟的是将要被抛出了异常的一个实例             |        throws 后面紧跟着的是要被抛出了异常的类的名字         |
| 4.      | 声明                                                         | 被用在方法里面                                               |                       被用在方法签名上                       |
| 5.      | 内部执行                                                     | 我们一次只能抛出一个异常，也就是说我们不能同时抛出多个异常   | 我们可以用throws关键字同时声明多个可能会被该方法抛出的异常，比如main() throws IOException, SQLException. |

**Java throw 例子：**

**TestThrow.java**

```java
public class TestThrow {  
    //defining a method  
    public static void checkNum(int num) {  
        if (num < 1) {  
            throw new ArithmeticException("\nNumber is negative, cannot calculate square");  
        }  
        else {  
            System.out.println("Square of " + num + " is " + (num*num));  
        }  
    }  
    //main method  
    public static void main(String[] args) {  
            TestThrow obj = new TestThrow();  
            obj.checkNum(-3);  
            System.out.println("Rest of the code..");  
    }  
}  
```

输出：

```te
Exception in thread "main" java.lang.ArithmeticException: 
Number is negative, cannot calculate square
	at Test_Lzw.exceptionTest.TestThrow.checkNum(TestThrow.java:7)
	at Test_Lzw.exceptionTest.TestThrow.main(TestThrow.java:16)
```

**Java throws 例子：**

**TestThrows.java**

```java
public class TestThrows {  
    //defining a method  
    public static int divideNum(int m, int n) throws ArithmeticException {  
        int div = m / n;  
        return div;  
    }  
    //main method  
    public static void main(String[] args) {  
        TestThrows obj = new TestThrows();  
        try {  
            System.out.println(obj.divideNum(45, 0));  
        }  
        catch (ArithmeticException e){  
            System.out.println("\nNumber cannot be divided by 0");  
        }  
          
        System.out.println("Rest of the code..");  
    }  
}  
```

输出：

```
Number cannot be divided by 0
Rest of the code..
```

**Java throw 和 throws 的例子：**

**TestThrowAndThrows.java**

```java 
public class TestThrowAndThrows  
{  
    // defining a user-defined method  
    // which throws ArithmeticException  
    static void method() throws ArithmeticException  
    {  
        System.out.println("Inside the method()");  
        throw new ArithmeticException("throwing ArithmeticException");  
    }  
    //main method  
    public static void main(String args[])  
    {  
        try  
        {  
            method();  
        }  
        catch(ArithmeticException e)  
        {  
            System.out.println("caught in main() method");  
        }  
    }  
}  
```

输出：

```te
Inside the method()
caught in main() method
```

