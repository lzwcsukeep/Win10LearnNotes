#### 多个源文件的c++ 程序

1.首先每个.cpp 的源文件，都经过预处理-> 编译 ->汇编 之后生成一个object file 。
如 a.cpp --》 a.o  ；b.cpp --》 b.o
2.链接器将所有的object file 链接成一个可执行文件（exectuable file ）

![](C:\Users\lzw\Desktop\img_src\c++程序编译链接过程.png)

#### 头文件与源文件

c++ 程序有一个显著特点，就是声明和定义的分离，换种说法也叫做接口和实现的分离。
头文件里存放各种声明，也就是接口
源文件里存放可执行代码和数据的定义，也就是实现。
头文件一般以.h 结尾。源文件以.cpp结尾。

#### c++ 程序的独立编译

因为c++ 程序一旦变大了，就需要独立编译。这样可以拆解任务，如果有修改也可以只修改一小部分，然后重编译被修改的文件即可。否则的话，尽管修改一点点，都需要重新编译整个文件。

#### 保持对象声明的一致性

一个解决声明一致性问题的简单方法就是在包含可执行代码和数据定义的源文件中，通过include 指令 include 含有接口信息的头文件。

即在source files 中 include header files .

source files 含有可执行代码和数据定义。

header files 含有类型声明这类接口信息。

头文件中可以include 其他头文件

#### c++程序中有一个唯一定义原则，也就是说一个对象object 在整个程序中，只能被定义一次

具体来讲就是，在所有的translation unit 中同一个对象只能有一次定义，但是可以有多次声明。

#### 更详细的c++ 程序构建过程

![](E:\Files\LearnNotes\img_src\c++_build_process.png)

[The C++ Build Process](https://faculty.cs.niu.edu/~mcmahon/CS241/Notes/build.html)
