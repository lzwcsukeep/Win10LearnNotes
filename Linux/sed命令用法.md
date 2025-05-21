# sed 命令格式

Normally `sed` is invoked like this:

```bash
sed SCRIPT INPUTFILE...
```

For example, to replace all occurrences of ‘hello’ to ‘world’ in the file input.txt:

```bash
sed 's/hello/world/' input.txt > output.txt
```

## 3.1 `sed` script overview

A `sed` program consists of one or more `sed` commands, passed in by one or more of the -e, -f, --expression, and --file options, or the first non-option argument if zero of these options are used. This document will refer to “the” `sed` script; this is understood to mean the in-order concatenation of all of the scripts and script-files passed in.

`sed` commands follow this syntax:

```bash
[addr]X[options]
```

X is a single-letter `sed` command. `[addr]` is an optional line address. If `[addr]` is specified, the command X will be executed only on the matched lines. `[addr]` can be a single line number, a regular expression, or a range of lines (see [sed addresses](https://www.gnu.org/software/sed/manual/sed.html#sed-addresses)). Additional `[options]` are used for some `sed` commands.

The following example deletes lines 30 to 35 in the input. `30,35` is an address range. `d` is the delete command:

```bash
sed '30,35d' input.txt > output.txt
```

总结：sed 命令的脚本也叫命令，格式为 `[addr]X[options]` 。地址表示范围，X 是命令。下面有地址的解释。

## 4.1 Addresses overview

Addresses determine on which line(s) the `sed` command will be executed. The following command replaces the word ‘hello’ with ‘world’ only on line 144:

```bash
sed '144s/hello/world/' input.txt > output.txt
```

If no addresses are given, the command is performed on all lines. The following command replaces the word ‘hello’ with ‘world’ on all lines in the input file:

```bash
sed 's/hello/world/' input.txt > output.txt
```

Addresses can contain regular expressions to match lines based on content instead of line numbers. The following command replaces the word ‘hello’ with ‘world’ only in lines containing the word ‘apple’:

```bash
sed '/apple/s/hello/world/' input.txt > output.txt
```

An address range is specified with two addresses separated by a comma (`,`). Addresses can be numeric, regular expressions, or a mix of both. The following command replaces the word ‘hello’ with ‘world’ only in lines 4 to 17 (inclusive):

```bash
sed '4,17s/hello/world/' input.txt > output.txt
```

ref: https://www.gnu.org/software/sed/manual/sed.html#sed-addresses

sed address表示命令作用的范围。范围可以用逗号隔开。

## sed 有模式空间(pattern space)的概念

所有匹配了的行，都是在模式空间内去执行相应的命令处理。

ref :

gnu sed 手册 ： https://www.gnu.org/software/sed/manual/sed.html

总结：

sed 作为流编辑器，逐行的处理文本。逐行处理文本时区分匹配还是不匹配(不写条件默认匹配),对于匹配的行会实行相应的命令，比如：

```bash
d：删除匹配行
a: 在匹配行后面增加一行或数行
i: 在匹配行前面增加一行或数行
s: 在匹配的行内执行替换，完整形式为： s/regex/replacement/flag 
p: 打印匹配行
```

然后对于匹配，可以用行号的数字直接指定具体的行，也可以使用正则表达式表示所有匹配了正则表达式的行，也可以使用`,(comma)` 分隔两个表示行的表达式表示匹配范围。表示行的表达式可以直接是数字，也可以是模式匹配，比如`3,10` 和 `/root/,/hello/`，前者表示3~10行(inclusion) ，后者表示从匹配了root的行开始到匹配了hello的行结束的范围。
匹配条件统一认为是一种地址(Address), 正则表达式也是被认为是一种地址。

总之，sed 格式看起来简单，但是能做到的功能挺强大的。

可以插入文本，删除文本，修改指定行的文本。配合正则表达式。
