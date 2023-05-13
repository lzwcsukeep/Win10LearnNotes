### vim 多标签编辑

```textile
vim -p files
```

![](C:\Users\lzw\AppData\Roaming\marktext\images\2022-07-19-22-28-40-image.png)

效果如下：

![](C:\Users\lzw\AppData\Roaming\marktext\images\2022-07-19-22-29-22-image.png)

以多标签同时打开多个文件，\* 表示当前目录所有文件

```textile
常用命令：
[n]gt - 切换到下一个标签。如果前面加了n,就切换到第n个标签。第一个标签的序号就是1。
wqa - 保存所有文件，退出vim
:tabc - 关闭当前标签，如果文件修改未保存，则vim会提示。
```

更多相关命令：

![](C:\Users\lzw\AppData\Roaming\marktext\images\2022-07-19-22-39-28-image.png)

### vim在当前光标下方插入其他文件内容或者外部命令输出结果

```textile
:r filename // 读取filename内容插入当前光标下方，可以插入filename的指定某几行 

:r !cmd  // 读取cmd 命令的输出结果到当前光标下方
```

### vim 在可视模式可以对高亮的文本进行operator 操作，还可以将高亮的文本按行保存进另一个文件。

```textile
可视模式选中文本高亮显示后执行
:w newfile
可以将高亮文本所在的行保存进 newfile中，注意是按行写入。
```

### vim :e [filename]命令

打开vim 之后，可以在vim 的命令行模式下 用`:e [filename]`  打开另一个文件。如果没有指定文件名，即`:e ` 时,vim重新打开当前文件，如果当前文件在vim 编辑的过程中被另一个程序修改了。那么`:e` 可以获取当前文件的最新版。

### vim 文件间拷贝

实现方法就是先编辑一个文件，用`y` 命令选中要拷贝的文本。然后用`:e anotherfile` 命令编辑另一个文件，在这个文件就可以直接用`p` 命令粘贴从上一个文件拷贝过来的内容了。

**如果需要从上一个文件的多处地方拷贝到另一个文件**，可以利用vim 的寄存器。把要拷贝的不同地方的文本放到不同的寄存器中，然后编辑下一个文件的时候，`p` 命令之前带上寄存器就行。

### vim 编辑多个文件

1. 打开某个文件file 的前提下，用`:e otherfiles` 命令编辑下一个文件(可重复使用`:e` 命令)，然后用`:bn ,:bp, :ls(buffers)`  命令在这些文件之前切换和查看文件位置。

2. 打开vim 的时候直接指定多个文件，这个时候叫做文件列表。即`vim one.c two.c three.c` 直接编辑多个文件。文件间切换用`:next  :previous :last(最后一个文件) :first(第一个文件)` 等命令。用`:args ` 命令查看当前文件在文件列表中的位置。

细节可以看vim 8.2 中文手册usr_07部分。

### vim !{motion}{filter} 的使用

![](C:\Users\lzw\Desktop\img_src\vim执行外部命令.png)

结果

![](C:\Users\lzw\Desktop\img_src\vim执行外部程序2.png)

也就是说将选中的几行作为输入给sort 程序，sort 程序的输出会替换原来选中的几行。

**"!!" 命令表示对当前行执行过滤命令。在unix 上"date" 命令可以打印当前时间日期，因此 "!!date<enter>" 命令会用"date"命令的输出替代当前行。这在为文件加入时间戳的时候非常有用。**
