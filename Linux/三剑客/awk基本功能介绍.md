# awk 的基本功能：

awk 的基本功能是在一个或者多个输入文件中查找符合 pattern 的行，并且执行相应的action。

# awk 的基本工作模式：

awk 会逐行读取命令行选项上给出的输入文件内容。

如果正在读取的文件的某行匹配到了某个 pattern ，就会执行与该pattern 对应的action. 如果一个rule 中的多个pattern 匹配到了，
那么这一行会触发这多个patterns 相对应的多个actions.

# awk 可以做什么？

- awk 里面可以自定义变量，而且可以对变量进行算术操作，因此awk 可以对文件进行统计分析，出一个简单的报告。
- awk 的 pattern 可以指定行号，可以使用正则表达式匹配字符串， action 可以打印。因此可以在文件中找出你想要的数据。
- awk 是按行读取的，一行看成一个记录，并且会自动将一行数据按照空格将一行划分成多个字段。因此你可以只打印你想要的字段。
从而进行精细化查看到字段。

重点： awk 适合对文件进行统计分析，出一个简单的报告。还可以在文件中找出你想要的数据，甚至可以只找到相应的字段。


-----

# awk 基本事实：

1. awk 命令使用的基本语法:

awk 'program' input-file1 input-file2 ... 

2. awk 的基本功能是在若干文件(files)中查找符合条件的行(lines)。

3. program 的概念：

一个awk program 由一系列的rule 组成，通常用Newline 隔开rules.
一条rule 由 pattern {action} 组成。

rule 里的pattern 和 action ：
rule 由 pattern + action 组成， pattern 是每一行匹配的字符串，action 是匹配到某行，对某行执行的动作。

因此一个awk program 看起来如下：
pattern { action }
pattern { action }
...
---
> The program consists of a series of rules (it may also contain function definitions, an advanced feature
that we will ignore for now). Each rule specifies one pattern to search for and one action to perform upon finding the pattern.
Syntactically, a rule consists of a pattern followed by an action. The action is enclosed
in braces to separate it from the pattern. Newlines usually separate rules. Therefore, an
awk program looks like this:
pattern { action }
pattern { action }
...
---

4. 输入文件的每一行都会执行所有的rules。

5. pattern 和 action 可以省略其中一个，但是不能同时省略。当省略pattern时默认每一行都匹配，当省略action 时默认action 是print。

注: 记住基本语法，是 awk 'program' input_files...

6. awk 里面的变量variable:
Variables in awk can be assigned either numeric or string values. The kind of value a variable holds can change over the life of a program. 
By default, variables are initialized to the empty string, which is zero if converted to a number. 
There is no need to explicitly initialize a variable in awk, which is what you would do in C and in most other traditional languages.

7. awk 会自动给每一行进行分段，叫fields,使用 `$数字` ，可以引用对应fields.特别的$0 表示整行记录。