## JAVA字节码文件结构(class file format)

JAVA源文件经javac 命令编译之后生成.class 文件，class 文件是一种和硬件以及操作系统无关的二进制格式文件。在JAVA 虚拟机规范中精确的定义了class 文件的格式。

认识class文件格式是很重要的，我们编译生成的字节码命令就保存在class 文件里。

首先 class file 由字节流组成，也就是一个一个字节组成。

其次class file 的组成部分可以分为两种

1. Item
2. Table

Table是复合结构，由0个或多个可变长度的Item组成 ，整个class file 可以看作一个Table .

从 ORACLE公司的官方文档JAVA虚拟机规范（JVMS）中截取的整个class 文件结构如下：

![](E:\Files\LearnNotes\img_src\class_file_stru.png)

先解释第一列：**u1，u2，u4** 表示一字节，二字节，4字节的无符号数，用来表示一个**Item**占用了多少个字节，其第二列是这个Item的名字。**cp_info，filed_info,meyhod_info**等表示这是一个Tbale，是个复合结构，Table的长度不是固定的，长度由其前面的 xx_count 指定,第二列是这个table 的名字。



所有结构翻译成中文为：

***magic***

魔数：魔数是一个Item,给定了一个数字来识别这是一个 class file 格式的文件。占四个字节，值为固定的0xCAFEBABE

***minor_version,major_version***

该class文件的次版本和主版本，这两个数合在一起来确定该class file 的版本。

***constant_pool_count*** 

常量池计数 Item,该Item的值等于常量池表中条目个数加一。为什么加一？ 因为常量池表索引0的位置保存NULL ，表示不引用任何数据类型。

***constant_pool[]***

这个结构是一个Table ,叫常量池表。该表表示了许多字符串常量值，类和接口名字，字段的名字以及引用class file 文件内其他结构和子结构的常量。既然是一张表，那么表中就有许多的条目，每个条目前都有一个数字作为tag，指示这个条目的类型。

常量池能保存的所有条目的类型为：

![cptag](E:\Files\LearnNotes\img_src\constant_pool_tag.png)

***access_flags***

是一个Item，指示这个类的访问标志

下图解释了各个访问标志

![](E:\Files\LearnNotes\img_src\access_flags.png)

***this_class***

这个Item 的值必须是常量池表中的一个有效索引，该索引指向常量池表中的条目一定是**CONSTANT_Class_info** 结构， **CONSTANT_Class_info**是常量池表能保存的结构之一，常量池能保存的所有结构类型在常量池表中给出。

***super_class***

对一个类来说，super_class Item 的值是0或者是常量池表中的一个有效索引。如果是非0,那么常量池表中该索引指向的类型一定是**CONSTANT_Class_info**表示该类的直接父类，**CONSTANT_Class_info**。如果是0，则这个类一定是Object类。只有Object 类没有直接父类。

***interfaces_count***

这个Item 给出了该类直接的父接口

***interfaces[]***

interfaces 数组中的每一个值都是常量池中的一个有效索引。常量池表中处于索引 *interfaces[i]* , *0 <= i < interfaces_count*,  位置的条目一定是一个**CONSTANT_Class_info** 代表该类的直接父接口。父接口顺出按照源码中从左到右的方式给出。

***fields_count***

字段计数器（fields_count） Item 给出了在字段表中(fields table)中 field_info 结构的数目。field_info 结构表示了所有的字段(field) 包括 类或接口中声明的类变量和实例变量。

***fields[]***

在fields table 中的每个值都是一个 field_info 结构,field_info结构给出了类或接口中一个字段的完成描述。field_info结构如图：

![image-20211110200448185](E:\Files\LearnNotes\img_src\fields_info_stru.png)

***methods_count***

方法计数器，这个Item类似field_count  ,给出了方法表(methods_table)中method_info 的数目

***methods[]***

方法表，类似于fields[] ，方法表中的每个值都代表了一个method_info 的结构，method_info 给出了类或接口中一个方法的完成描述。

method_info 结构如图所示：

![image-20211110201214281](E:\Files\LearnNotes\img_src\method_info_stru.png)

***attributes_count***

属性计数器(attributes_count) 给出了属性表中（attributes table）属性的个数

***attributes[]***

属性表中的每个值都是一个*attribute_info* 的结构。jvms11所定义的所有属性种类按位置排序如下表所示： 

![image-20211110201928147](E:\Files\LearnNotes\img_src\attributes_kind.png)

而所有的attribute_info的通用格式如下所示：

![image-20211110202203752](E:\Files\LearnNotes\img_src\attribute_info_stru.png)

至此，一个class 文件所蕴含的结构都列举出来了，在知道整个class 格式大框架的条件下再去研究每种结构的内部细节，其中比较重要的就是常量池表，字段表和方法表了吧。

上面这种列举的方法实在是有些死板，但是没办法，文件格式这种的东西就是相对固定，没有什么理解的东西。不过介绍两款工具可以让我们实际的看看class 文件长啥样。

- **jclasslib bytecode viewer**    //类似反编译的字节码查看工具，这个可以作为软件单独下载，也可以在IDEA插件市场中作为插件下载
- **binary viewer**   //16进制形式查看

我在Win10上编写了如下的Test.java源码

```java
public class Test{
	private int id ;
	private String name;
	public static void main(String[] args) {
		int i=2 ;
		int j = 3;
		int k=i+j;	
	}
	public int getID(){
		return id ;
	}
}
```

进入Test.java 所在目录，执行`javac Test.java` 命令，可以看到当前目录会生成一个Test.class 文件

![image-20211110204600260](E:\Files\LearnNotes\img_src\TestForJclasslib.png)

安装好Jclasslib bytecode viewer 和 binary viewer ，并分别用它们打开 Test.class 文件

- jclasslib

![image-20211110205129809](E:\Files\LearnNotes\img_src\jclasslibOpenTest.png)

我用的JDK版本是11，因此major version number就是55了，55就代表jdk11。然后左侧还有一些其他的东西，下载软件，自己编写一个简单的文件，编译之后用jclasslib打开之后都可以点点，然后对比自己源码看看。我使用之后的感觉是java 虚拟机规范里对class文件格式所定义的部分，jclasslib里面都能找得到对应的部分。

- binary viewer

![image-20211110210049906](E:\Files\LearnNotes\img_src\binaryviewerOpenTest6.png)



binary viewer 就比较硬核了，它是直接以16进制格式查看字节码文件。按照java 虚拟机规范的内容，对照图中圈1，圈2，圈3，分别是魔数(magic)、次版本号(minor_version)、主版本号(major_version)。是不是和jclasslib里面显示的一致呢？你如果时间够，还可以更硬核的将binary viewer 里所有的字节对照java虚拟机规范，逐个逐个翻译出来看看是不是符合其格式定义。你会发现真的是一样的！！！！



- 在IDEA插件市场中安装jclasslib
  - 打开设置，进入插件，搜索框搜索jclasslib即可

![image-20211110212708373](E:\Files\LearnNotes\img_src\ideaJclasslibInstall.png)

安装好之后选中编译好的class 文件

点View --> show Bytecode with Jclasslib 即可

![image-20211110213448134](E:\Files\LearnNotes\img_src\222.png)

效果如图

![image-20211110214318468](E:\Files\LearnNotes\img_src\333.png)
