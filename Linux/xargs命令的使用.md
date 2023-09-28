xargs 命令是extended arguments 的缩写，意为扩展的参数。

语法格式：

`xargs [options] [utility [argument] ... ]`

xargs 命令行工具可以构建一个命令行并执行，该命令行由utility 和 argument 组成并且argument后面还会跟随着从标准输入序列中读入的尽可能多的参数作为额外参数。xargs命令然后调用并且等待构建好的命令行执行完毕。

上述流程一直重复，直到下面一种情况发生：

- 一个 `end of line`  从标准输入被检测到

- The logical end-of-file string (see the -E eofstr option) is found on standard input after double-quote  processing,  apostrophe  processing,  and  backslash escape processing (see next paragraph).

- An invocation of a constructed command line returns an exit status of 255.

> The  application  shall  ensure  that arguments in the standard input are separated by unquoted <blank>s, unescaped <blank>s, or <newline>s. A string of zero or more non-double-quote ( ' )' characters and non- <newline>s can be quoted by enclosing them in double-quotes. A string of zero or more  non-apostrophe  (  '"  ) characters and non- <newline>s can be quoted by enclosing them in apostrophes. Any unquoted character can be escaped by preceding it with a backslash. The utility named by utility shall be executed one or more times until the end-of-file is reached or the logical end-of file string is found. The results  are  unspecified if the utility named by utility attempts to read from its standard input.
> 
> 这个应用程序应该确保从标准输入读取的参数被`没有引号括起来的空格`，`没有被转义的空格`或者`换行符`分隔。

简要说就是xargs可以将标准输入转化成另一个命令的参数。默认是跟在原有参数的后面。并且从标准输入读取的参数个数大于另一个命令的参数上限或者用`-n num` 指定的数字时，会重复构建命令行，并再次执行。

#### 为什么要用xargs？

因为有些linux命令不支持从标准输入读取内容作为参数，用xargs 就可以将标准输入转化为命令的参数。

#### 常用用法：

- echo "hello world" | xargs echo
  
  - output : hello world

- echo "hello world" | xargs -n 1 

output:

```c
hello
world
```

这是因为`-n 1` 指定了一次只传一个参数，而echo "hello world" 参数的参数以空格分开，有hello和world两个参数。因此执行了两次echo。一次是echo hello,另一次是echo world。

当xargs 不指定要执行的命令时默认是echo 命令。

- 可以用`-p` 提示最后执行的命令是什么并选择是否执行，yes or no .

- echo "hello world" | xargs -n 1 -p

```c
echo hello ?...y
echo world ?...hello
n
//输入n 时第二个命令没有执行。
//输出顺序有点乱了，但是不影响理解。
```

- 用`-t` 参数输出执行的命令，不需要用户确认。t 表示 trace. 

- echo "hello world" | xargs -t -n 1 echo

输出

```c
echo hello 
hello
echo world 
world
```

使用`-I` 加占用符，指定从标准输入读取的参数在另一个命令行中的位置。

- `echo "hello world" | xargs -I _ echo this is _ sentence. `   // _ 为占用符

output:

```c
this is hello world sentence.
```

占用符可能可以任意指定，我用数字1都可以

```c
[root@centosvm136 ~]# echo "hello world" | xargs -I 1 echo this is 1 sen.
this is hello world sen.
```
