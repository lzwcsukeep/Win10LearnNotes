### vim 进阶笔记

#### 1.指定范围替换

可以指定确定的行或行范围进行替换，具体如下

10.3    Command ranges

The ":substitute" command, and many other : commands, can be applied to a
selection of lines.  This is called a range.
   The simple form of a range is {number},{number}.  For example:

        :1,5s/this/that/g

Executes the substitute command on the lines 1 to 5.  Line 5 is included.
The range is always placed before the command.

A single number can be used to address one specific line:

        :54s/President/Fool/

#### 2.寄存器的使用

```textile
"fyas   //拷贝一个句子到f寄存器
"l3Y    //拷贝三个整行到l寄存器
```

现在有了在两个寄存器f ，l  的两段文本，移动到另一个文件(也可以是本文件)想要插入文本的地方：

```textile
"fp  // 将f寄存器的内容粘贴到当前行下方。
```

#### 3.宏录制

1. q+字母({register}) 启动宏录制。结果保存到{register}指定的寄存器中。{register}可以是a-z中任一字母

2. 输入命令

3. 键入q结束宏录制。

录制完毕用"@+字母"命令执行这个宏

#### 4.标记文件位置

1. 命令m+字母。即可标记当前光标下的位置。

2. 使用 **\`+字母** 跳到该字母标记的位置。

3. 使用 **'+字母** 跳到该字母标记的行首的位置。

对于步骤一，使用小写字母标记时只能在当前文件内跳转，使用大写字母标记时，可以在文件之间跳转。即大写字母属于全局标记。小写字母属于局部标记。

    
