### Java中的类加载机制(ClassLoader in Java)

**为什么存在或需要类加载机制？**

>用静态语言编写的程序(如C，C++)会被编译成与机器相关的指令，并且保存为一个可执行文件，将不同代码文件组合成一个可执行的本地代码的过程叫做链接-有着共享库代码但是分别编译的不同源代码的合并操作，目的是产生一个可执行文件。
>
>在动态编译语言中如Java中，这是不同的。在Java中.class 文件由 java编译器编译生成，在运行时根据需要被加载进JVM中，也就是说JAVA的链接操作是在运行时被执行的。如果被加载的类同时依赖于另一个类，那么该类也会被加载。
>
>也就是说，在java程序运行过程，如果需要用到某个类，则必须先将该类加载进JVM。

**什么时候会加载类？**

只有两种情况：

- 当 `new` 字节码被执行的时候
- 当字节码执行了一个类的静态引用，比如 `System.out` 



#### JAVA的类加载器类型

并不是所有的类都用一个类加载器类加载的，根据类的类型以及路径，会采用不同的类加载器。所有的类都是基于其类名来加载的，如果该类没有找到就返回一个`ClassNotFoundException` 或者 `NoClassDefFoundError`



JAVA有三种类加载器：

1. **BootStrap ClassLoader :** 

   > Bootstrap ClassLoader loads classes from the location ***rt.jar\***. Bootstrap ClassLoader doesn’t have any parent ClassLoaders. It is also called as the **Primodial ClassLoader**.

2. **Extension ClassLoader:**

   >The Extension ClassLoader is a child of Bootstrap ClassLoader and loads the extensions of core java classes from the respective JDK Extension library. It loads files from ***jre/lib/ext\*** directory or any other directory pointed by the system property ***java.ext.dirs\***.

3. **Application ClassLoader:**

   > Application ClassLoader loads the Application type classes found in the environment variable ***CLASSPATH, -classpath or -cp command line option\***. The Application ClassLoader is a child class of Extension ClassLoader.

#### 类加载器的三个原则：委派，可见性，唯一性（**Delegation**, **Visibility**, and **Uniqueness**）

- **Delegation principle:** 将加载请求向前提交给父加载器 ，只有当父加载器没有找到或加载该类时才加载该类
- **Visibility principle: ** 该原则允许子加载器可以看见所有被父加载器加载的类，但是父加载器无法看到子加载器加载的类
- **Uniqueness principle: ** 一个类只允许被加载一次。该原则被**Delegation principle** 实现。其确保子加载器不会再次加载父加载器已经加载过的类。









#### 委派模型

委派模型是加载一个类的流程

- 将JVM需要使用一个类时，加载类的请求先送到**Application ClassLoader** ，**Application ClassLoader** 将请求委派给**Extension ClassLoader** ，**Extension ClassLoader** 继续委派给**BootStrap ClassLoader**  
- **BootStrap ClassLoader** 在rt.jar中查找是否有该类，有就加载该类，如果没有就将请求转移给*Extension ClassLoader*，同理 *Extension ClassLoader* 收到请问后在其搜索路径jre/lib/ext 中查找该类，有就加载该类，如果没有就将请求转移给*Application ClassLoader* ，*Application ClassLoader* 收到请求就在CLASSPATH中查找该类，如果找到就将其加载进内存，没有就报*ClassNotFoundException*异常。

![rule](E:\Files\LearnNotes\img_src\rule2.png)



**类加载流程图：**

![jvmclassloader](E:\Files\LearnNotes\img_src\jvmclassloader.jpg)



参考链接：

1. <https://www.geeksforgeeks.org/classloader-in-java/>
2. <https://www.javatpoint.com/classloader-in-java>
3. <https://www.oracle.com/technical-resources/articles/javase/classloaders.html>
4. 直接Google搜索 what is classloader in java?

