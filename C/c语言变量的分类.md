#### C语言变量的分类

C语言和Java 变量的分类有一点区别，关键字static 用来修饰变量时其含义也不一样。

---

**自动变量：** 在K&R那本书中和局部变量同义，指的是定义在函数内部的变量。只能在定义自动变量的函数内可见，其余的函数无法直接访问它们。自动变量只在函数调用期间存在，每次进入函数需要显示的赋值，如果没有赋值，则其中存放的是无效值。

**外部变量：** 定义在所有函数外部的变量。外部变量在程序执行期间一直存在。外部变量必须定义在所有函数之外，并且只能定义一次。定义后编译程序将为其分配存储单元。在每个需要访问外部变量的函数中，必须声明相应的外部变量，此时说明其类型。从语法角度看，外部变量的定义和局部变量的定义是相同的，但由于它们位于各函数的外部，所以它们是外部变量。

---

**关于外部变量的extern 声明：**

在一个源程序的所有源文件中，一个外部变量只能在某个文件中定义一次，而其他文件可以通过extern 声明来访问它（定义外部变量的源文件也可以包含对该外部变量的extern 声明）。外部变量的定义中，必须指定数组的长度，但是extern声明则不一定要指定数组的长度。

在源文件中，如果外部变量的定义出现在使用它的函数之前，那么在那个函数中就没有必要使用extern 声明。

如果要在外部变量的定义之前使用它，则需要extern 声明。

如果程序包含在多个源文件中，而某个变量在file1中定义，而在file2,file3中使用，那么在file2,file3中就需要extern 声明来建立改变量与其定义之间的联系。

**这里必须严格区分外部变量的定义和声明。变量的声明用来说明变量的属性（主要是变量的类型），而变量的定义还将引起存储单元的分配。**

**静态变量(static)：**某些外部变量，仅在其所在的源文件中的函数使用，其他函数不能访问。这时候可以在变量前用**static**关键字修饰。**用static声明限定外部变量和与函数，可以将其后声明的对象作用域限定为被编译源文件的剩余部分。通过static 限定外部对象，可以达到隐藏外部对象的目的。**

**外部的static 声明也可用于函数，**如果函数用static声明，则该函数名仅在声明所在的文件可见，其它文件都无法访问。

**static 也可用于声明内部变量。**static 类型的内部变量，同自动变量一样，只能在函数内使用，但它与自动变量不同的是，不管其所在函数是否被调用，它一直存在，而不像自动变量那样，随着所在函数的被调用和退出而存在或消失。换句话说，static 类型的内部变量是一种只能在某个特定函数中使用，但一直占据存储空间的变量。

**寄存器变量：**register 声明告诉编译器，它声明所在的变量在程序中使用频率较高，其思想是将register 变量放在机器的寄存器中，这样可以使程序更小，执行速度更快，但是编译器可以忽略此选项。

register 声明的形式如下：

```c
register int x;
register char c;
```

register 声明只适用于自动变量以及函数的形式参数。

下面是后一种情况的例子：

```c
f(register unsigned m, register long n)
{
    register int i;
    ...
}
```
