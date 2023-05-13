## Java throws 关键字

Java throws 关键字被用来声明一个异常。它提示程序员这里可能会产生一个异常，因此最好提供一个异常处理以维持程序的正常流。

异常处理主要是为了处理检查型异常的。如果发生了非检查型异常，比如空指针异常，通常是由于程序员没有仔细检查自己的代码导致。

**throws 的使用语法**

```java
return_type method_name() throws exception_class_name{  
//method code  
} 
```

**哪些异常需要被声明？**

只有**检查型异常（checked exception）**需要被声明，因为：

- **非检查型异常(unchecked exception)：**在我们的控制之下因此我们可以改正代码
- **错误(Error)：**超出我们的控制，比如我们无法做任何事，如果发生了栈溢出或者虚拟机错误。



**throws 关键字的好处**

使得检查型异常（checked exception）也可以在调用栈中传递。

它给方法的调用者提示存在的某异常的信息。

**throws 关键字的例子**

让我们看一下throws 的例子，它展示了检查型异常通过throws 关键字可以在调用栈中传递。

**Testthrows1.java**

```java
import java.io.IOException;  
class Testthrows1{  
  void m()throws IOException{  
    throw new IOException("device error");//checked exception  
  }  
  void n()throws IOException{  
    m();  
  }  
  void p(){  
   try{  
    n();  
   }catch(Exception e){System.out.println("exception handled");}  
  }  
  public static void main(String args[]){  
   Testthrows1 obj=new Testthrows1();  
   obj.p();  
   System.out.println("normal flow...");  
  }  
}  
```

输出：

```tex
exception handled
normal flow...
```

规则：如果我们调用了一个声明了异常的方法，我们必须捕捉它或者声明这个异常。

**有两种场景：**

1.场景一：我们捕获了这个异常，也就是我们用try-catch 语句块处理了这个异常

2.场景二：我们声明了这个异常，也就是我们将throws关键字加在方法上

**场景一例子：用try-catch捕获异常：**

在我们处理异常的情况下，无论异常是否在程序中发生，代码都会被很好地执行。

**Testthrows2.java**

```java
import java.io.*;  
class M{  
 void method()throws IOException{  
  throw new IOException("device error");  
 }  
}  
public class Testthrows2{  
   public static void main(String args[]){  
    try{  
     M m=new M();  
     m.method();  
    }catch(Exception e){System.out.println("exception handled");}     
  
    System.out.println("normal flow...");  
  }  
}  
```

输出：

```te
exception handled
       normal flow...
```

**场景二例子：声明异常：**

- 如果我们声明异常，异常没有发生，程序会良好的执行
- 如果我们声明异常然后异常发生了，它会在运行时被抛出，因为throws 关键字没有处理这个异常

让我们看看上述两种场景的例子：

**A) 如果异常没有发生**

**Testthrows3.java**

```java
import java.io.*;  
class M{  
 void method()throws IOException{  
  System.out.println("device operation performed");  
 }  
}  
class Testthrows3{  
   public static void main(String args[])throws IOException{//declare exception  
     M m=new M();  
     m.method();  
  
    System.out.println("normal flow...");  
  }  
}  
```

输出：

```te
device operation performed
       normal flow...
```

**B）如果产生异常**

**Testthrows4.java**

```java
import java.io.*;  
class M{  
 void method()throws IOException{  
  throw new IOException("device error");  
 }  
}  
class Testthrows4{  
   public static void main(String args[])throws IOException{//declare exception  
     M m=new M();  
     m.method();  
  
    System.out.println("normal flow...");  
  }  
}  
```

输出：

```te
Exception in thread "main" java.io.IOException: device error
	at Test_Lzw.exceptionTest.M.method(Testthrows4.java:5)
	at Test_Lzw.exceptionTest.Testthrows4.main(Testthrows4.java:11)
```

