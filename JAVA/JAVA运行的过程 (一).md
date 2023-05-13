### Java程序运行底层分析

我们写了很多java代码，并且在IDE上可以很方便的运行它，那么有没有人想知道，这个java程序在计算机底层世界是如何运行的呢？

比如我们在IDE或者编辑器上编写了如下一个简单的java文件，命名为Test.java

```java
public class Test{
	public static void main(String[] args){
		System.out.println("Hello World!");
	}
}
```

进入到Test.java所在目录按如下步骤运行它：

- javac Test.java     //编译生成字节码文件Test.class 
- java Test               //运行该字节码文件

会在控制台有如下输出：

![](E:\Files\LearnNotes\img_src\TestOut.png)

那么从更底层来看，这个JAVA程序是如何运行的呢？

我们都知道

1. JAVA程序在JVM上执行，JVM执行的是编译后字节码文件
2. 计算机只认识二进制的01，程序要执行得转化成机器码
3. 字节码是一种中间代码，不是最终运行的机器码

那么就有疑问了，上述字节码文件又是如何最终在机器上运行的呢？这个机器不是指JVM，而是物理机。

首先看下图：

![Javac](E:\Files\LearnNotes\img_src\javadebug.gif)

该图如果学过编译原理的可能一下子就看明白了，C/C++程序的编译流程几乎和这个一样，只不过这个编译的结果C/C++生成的是机器码，而JAVA程序生成的是

中间代码-字节码文件，C/C++程序到这步就可以直接执行了，但是JAVA程序还不行。

上图所对应的命令其实就是 `javac Test.java` 命令生成了`Test.class` 文件，所以我们一个简单的`javac` 命令其实背后的流程可不简单。

上述过程的细节是一门大学问，从**词法分析**到**语法分析**再**语义分析**再生成**中间代码**，然后可能还需要**优化**，最终生成**目标代码**。

词法分析的内容比如，我写了代码

```java
if (ifx>5){}
```

计算机怎么知道第一次把 i和f 放在组成if关键字 而后面就把i ,f ,x放在一起作为一个变量，而不是看成if + x 呢？计算机只认识01啊。这里面的学问就在编译原理里面。

有兴趣的可以去找找编译原理的视频和书籍看，B站就有，学了对理解源文件.java /.c/.cpp 是如何转化成机器代码有巨大帮助。有句话说的好，其实我们写高级语言就是在讨好编译器。

不想深入的我们只要知道有这些流程，每个流程干了啥，流程的输出是什么就好了。

上图，我们知道了.java 文件 如何生成  .class 文件。但是离真正在机器上执行还是有一段距离。

这段距离由JVM来走完。看下图

![just-in-time](file://E:\Files\LearnNotes\img_src\jvmdebug.gif)



JVM执行引擎分为两部分，JIT编译器和字节码解释器，JIT编译器会识别热点代码（也就是执行频繁的代码）将其编译成机器代码，以提高执行效率。

字节码解释器的任务是将字节码逐条翻译为机器码。

为了理解，可以简单的演示一下java程序是如何转化成字节码的。

- 编写下面的Java代码，保存为TestADD.java

```java
public class TestADD{
 public static void main(String[] args){
	int i=2;
	int j=3;
	int k=i+j ;
	}
}
```

- javac TestADD.java      //编译生成 .class文件
- javap -v TestADD.class      //反编译查看.class 文件

结果如图：

![](E:\Files\LearnNotes\img_src\TestADDOut.png)

可以看到源文件main 方法内容

```java
{
    int i=2;
	int j=3;
	int k=i+j;
}
转化成字节码指令为：
         0: iconst_2    //常量2
         1: istore_1    //i
         2: iconst_3    //常量3
         3: istore_2    //j
         4: iload_1     //2 入栈
         5: iload_2     //3 入栈
         6: iadd        // 栈顶两个数相加得5
         7: istore_3    //保存到 k
         8: return      //main 方法结束
```

然后字节码指令 

`iconst_2` 、`istore_1` 、`iload_1` 、`iadd` 、`return ` 等这些都属于**JVM指令集**，这些指令是基于栈架构的，最终需要翻译成**对应机器的机器指令**。

而每一条字节码指令翻译成相应机器上的机器指令的翻译工作由JVM执行引擎来做，我们不需要关心。

而且字节码指令集在不同的机器上都是一样的，不同的只是在不同的机器上JVM会翻译成不同的机器指令。因此用户只需要面向JVM编写代码就行，而不用关心这个代码将在哪个机器上跑，这也是java跨平台的关键。



请大家批评指正！