# make 命令有一个 Option 可以打印make构建时使用的规则
from man make:

-p, --print-data-base
            Print the data base (rules and variable values) that results from reading the makefiles; then execute as usual  or  as  otherwise  specified.   This  also
            prints the version information given by the -v switch (see below).  To print the data base without trying to remake any files, use make -p -f/dev/null.

在 make 中，可以通过内置的命令行选项查看所有隐式规则的具体实现，常用的方法如下：

1. 使用 make -p 查看所有隐式规则和变量
-p 选项会让 make 打印出所有内置规则、变量定义以及用户定义的规则（如果有的话），其中就包含 .c 到 .o、.o 到可执行文件等隐式规则的具体实现。
执行命令：

```bash
make -p > implicit_rules.txt  # 将输出重定向到文件，方便查看
```

在输出中搜索关键字（例如 .o: 或 .c.o），可以找到 .c 编译为 .o 的隐式规则，类似如下内容：

```makefile
# 隐含规则搜索链：.c.o 表示从 .c 生成 .o
%.o: %.c
#  命令以Tab开头（此处为了显示用空格，实际是Tab）
    $(COMPILE.c) $(OUTPUT_OPTION) $<
```

其中：

% 是通配符，匹配任意文件名（例如 foo.o 对应 foo.c）。
$(COMPILE.c) 是一个预定义变量，展开后包含编译器和编译选项（默认是 $(CC) $(CFLAGS) $(CPPFLAGS) -c）。
$(OUTPUT_OPTION) 默认是 -o $@（$@ 表示目标文件，即 .o 文件）。
$< 表示第一个依赖文件（即 .c 源文件）。

2. 结合 make -f /dev/null -p 只看内置隐式规则
如果当前目录有 Makefile，make -p 会同时输出用户定义的规则，可能干扰查看。可以用 -f /dev/null 让 make 不读取任何 Makefile，只打印内置规则：

```bash
make -f /dev/null -p | grep -A 5 "%.o: %.c"  # 过滤出 .c 到 .o 的隐式规则
```
输出会更简洁，直接展示 make 内置的 .c 到 .o 规则细节。

3. 查看特定类型的隐式规则
如果只关心某类隐式规则（例如 C++ 的 .cpp 到 .o），可以结合 grep 过滤关键字，例如：
```bash
# 查看 C++ 源文件编译为 .o 的隐式规则
make -f /dev/null -p | grep -A 5 "%.o: %.cpp"
```
总结
通过 make -p（或 make -f /dev/null -p）可以直接查看所有隐式规则的具体实现，其中 .c 到 .o 的规则核心是通过预定义变量 COMPILE.c 展开为编译器命令，配合自动变量（$@、$< 等）完成编译。

重点： 1. 使用make -p 选项可以打印出使用的规则和变量，
      2. 只看内置规则，不构建使用 make -p -f/dev/null.
      3. 输出重定向到文件方便查看，结合 grep 查看特定规则