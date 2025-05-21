# How sed Works

Sed 的内部工作原理：
sed maintains two data buffers: the active pattern space, and the auxiliary hold space. Both are initially empty.

sed operates by performing the following cycle on each line of input: first, sed reads one line from the input stream,
removes any trailing newline, and places it in the pattern space. Then commands are executed; each command can have an address associated to it: addresses are a kind of condition code, and a command is only executed if the condition is verified before the command is to be executed.

When the end of the script is reached, unless the -n option is in use, the contents of pattern space are printed out to the output stream,
adding back the trailing newline if it was removed.8 Then the next cycle starts for the next input line.

Unless special commands (like ‘D’) are used, the pattern space is deleted between two cycles. The hold space, on the other hand,
keeps its data between cycles (see commands ‘h’, ‘H’, ‘x’, ‘g’, ‘G’ to move data between both buffers).

也就是说在不涉及 Hold space 的情况下，模式空间经过 script(一个个的commands作用到当前正在处理的行) 处理之后的行会自动打印出来。
使用`-n` command line option 可以压制这种默认输出。 然后在 commands 里面有一个p command 可以手动打印当前的模式空间。使用p
命令打印模式空间内容的时候不属于sed工作模式的自动打印。

举例，运行下面的命令，输出如下：

```bash
[lzw@ubun ~/temp/learn]$ sed "2p" main.cpp
This is added line
stdio.h end !!!!
stdio.h end !!!!
unistd.h change1 end !!!!
stdlib.h c2 end !!!!  
Note the difference between using a movement command and an object.  The end !!!!
movement command operates from here (cursor position) change2 to where the movement end !!!!
takes us.  When using an object the whole object is operated upon, no matter end !!!!
where on the object the cursor is.  For example, compare "dw" and "daw": "dw" end !!!!
deletes from the cursor position to the start of the next word, "daw" deletes c3 end !!!!
the word under the cursor and the space after change3 or before it.
    /* ~ This is a test ~ of the text formatting. ~
	 */ 

```
其中第二行 `stdio.h end !!!!` 输出了两次，就是因为一次是执行p command 的手动输出,一次是pattern space 的自动输出。
当script 中的commands 一执行完毕就会自动打印当前模式空间中的内容。所以这种情况是先执行的 p 命令，再自动打印当前模式空间的内容。
