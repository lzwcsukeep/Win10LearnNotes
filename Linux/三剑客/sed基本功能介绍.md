# sed 命令的定位

sed 命令叫做流编辑器，既然是编辑器，其主要定位就是用来编辑文件的。只不过通常的编辑器需要打开一个面板进行手动编辑，而 sed 可以做到在命令行上编辑文件。

sed 的定位是一个编辑器，编辑器的基本功能就是增删改某一行或者某些行数据。

sed 功能就是用来增删改查文件的。

sed 的通常语法：

`sed SCRIPT INPUTFILE...`

关于 sed 有没有 -e 选项的解释：

Without -e or -f options, sed uses the first non-option parameter as the script, and the following non-option parameters as input files. 
If -e or -f options are used to specify a script, all non-option parameters are taken as input files.
也就是说一般需要-e 参数指定script, 但是没有-e 或者-f ，sed 会自动将第一个非option 的参数当作script.

sed 里面可以指定地址(行号是一种地址)，/pattern/ 这种是被当作一种地址来看待的。 from man sed.

sed 里面修改某一行的命令有i,a,c。这几个命令会跟一下text,有两种等价的形式,以a为例
```bash
a\
text
Append text after a line.

a text
Append text after a line (alternative syntax).
// 上述两种是一样的。
```

sed 中的r和R command:
r和R 命令跟一个文件名file_name，用空格隔开，可以读取文件file_name的内容一以append方式追加到当前行后面，r和R 命令是 0地址或1地址的命令，即该命令前可以给一个地址(optional)。
r 命令读取整个文件file_name的内容，R读取文件的一行。

当不指定地址的时候，r命令默认每一行都追加file_name 的内容。R命令一次读取一行，追加到当前行后面(已经读取的行不会再次读取，会读file_name文件中的已读行的下一行)，直到file_name文件读取完毕。

Refs:
[GNU Sed Overview](https://www.gnu.org/software/sed/manual/sed.html#Overview)

或者直接Google "sed linux"

## sed 需要澄清的几个概念

1. sed 使用的基本语法：`sed SCRIPT INPUTFILE...` ，完整的使用语法是 `sed OPTIONS... [SCRIPT] [INPUTFILE...]`

2. script 的概念：

script 里面是执行的脚本，script 由 one or more commands 组成。

command 的结构是：[addr]**X**[options]

**X** is a single-letter sed command. [addr] is an optional line address. If [addr] is specified, the command X will be executed only on the matched lines.  
[addr] can be a single line number, a regular expression, or a range of lines (see sed addresses). Additional [options] are used for some sed commands.
The following example deletes lines 30 to 35 in the input. 30,35 is an address range. d is the delete command:

sed '30,35d' input.txt > output.txt
使用 -e 或者 -f 选项可以添加 script 到最终执行的script 里面，如果有多个-e 或 -f (或者-e 和 -f 同时存在)，那么最终执行的script 相当于 -e或-f 选项给出的script 的拼接。
> Options -e and -f can be combined, and can appear multiple times (in which case the final effective script will be concatenation of all the individual scripts).
> Commands within a script or script-file can be separated by semicolons (;) or newlines (ASCII 10). Multiple scripts can be specified with -e or -f options.
下面的例子完全等价：
The following examples are all equivalent. They perform two sed operations: deleting any lines matching the regular expression /^foo/, and replacing all occurrences of the string ‘hello’ with ‘world’:

sed '/^foo/d ; s/hello/world/' input.txt > output.txt

sed -e '/^foo/d' -e 's/hello/world/' input.txt > output.txt

echo '/^foo/d' > script.sed
echo 's/hello/world/' >> script.sed
sed -f script.sed input.txt > output.txt

echo 's/hello/world/' > script2.sed
sed -e '/^foo/d' -f script2.sed input.txt > output.txt

4. 地址的概念

sed 中的command 前面可以带地址，0个，1个或两个。地址可以是直接给出行号，也可以是正则表达式。两个地址用逗号隔开表示一个地址范围（add1,add2）。`$`符表示最后一行。
如果命令带了地址，那么命令command 只在给定的地址上生效，没给地址就是在所有的行上生效。

5. 多个commands 可以用大括号放在一起 { comand1;command2;...}，看成一个整体。

## sed 基本事实：

1. script 由一个或多个commands 组成。

> -e 后面跟的是script ，可以用多个-e 选项引入多个script. 
   -f 后面跟的是含有script 的文件，也可以用多个-f 选项引入多个script 文件
   -e 和 -f 可以同时出现，即混杂使用。
多个-e 或 -f 的最终效果是这些script 的commands 拼接在一起。

2. command 的 结构是: [addr]X[options]
X 是单个字符的命令。

3. sed 常用的command list:
a c i d p s r w...
其中 s 是替换命令，最实用。
s 命令的完整语法是：s/regexp/replacement/flags

完整command lists 链接：
[sed-commands-lists](https://www.gnu.org/software/sed/manual/sed.html#sed-commands-list)

## sed 的命令行options

最常用的是`-i` option 表示就地改变源文件的内容。

sed -i 's/hello/world' input.txt
将input.txt 中的hello 替换成world 并写入源文件。
