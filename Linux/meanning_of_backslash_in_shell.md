简言之：
反斜杠在Linux命令行中可以将一条命令分成多行。
也就是说，如果一条命令太长，命令行一行写不下，就可以在末尾使用反斜杠，在下一行续上之前的命令。
看下面英文描述：
If you are typing a long command that will not fit on the command line, you can use the \ (backslash) continuation character at the end of the first line. When you then press <Enter>, the command line is cleared so that you can continue typing. The line you typed prior to the backslash is displayed in the output area, and beneath it the shell prompt changes to > to indicate that you are continuing a command. For example:

```shell
$ cat /usr/macneil/uts/mydir/mydata\
>

```


