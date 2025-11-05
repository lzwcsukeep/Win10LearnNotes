### makefile 定义变量

直接变量名 = [变量值]

比如 

```makefile
objects = main.o kbd.o command.o display.o \
     insert.o search.o files.o utils.o
```

objects 就等于 后面一长串东西。使用方式 `$(objects)` 。

以后脚本中变量名出现的地方都会以**文本替换**的方式替换成变量值。

### 包含其他的Makefile

在Makefile使用 `include` 指令可以把别的Makefile包含进来，这很像C语言的 `#include` ，被包含的文件**会原模原样的放在当前文件的包含位置**。 `include` 的语法是：

```shell
include <filenames>...
```

`<filenames>` 可以是当前操作系统Shell的文件模式（可以包含路径和通配符）。

举个例子，你有这样几个Makefile： `a.mk` 、 `b.mk` 、 `c.mk` ，还有一个文件叫 `foo.make` ，以及一个变量 `$(bar)` ，其包含了 `bish` 和 `bash` ，那么，下面的语句：

```makefile
include foo.make *.mk $(bar)
```

等价于：

```makefile
include foo.make a.mk b.mk c.mk bish bash
```

make命令开始时，会找寻 `include` 所指出的其它Makefile，并把其内容安置在当前的位置。就好像C/C++的 `#include` 指令一样。如果文件都没有指定绝对路径或是相对路径的话，make会在当前目录下首先寻找，如果当前目录下没有找到，那么make还会在下面的几个目录下找：

1. 如果make执行时，有 `-I` 或 `--include-dir` 参数，那么make就会在这个参数所指定的目录下去寻找。

2. 接下来按顺序寻找目录 `<prefix>/include` （一般是 `/usr/local/bin` ）、 `/usr/gnu/include` 、 `/usr/local/include` 、 `/usr/include` 。

环境变量 `.INCLUDE_DIRS` 包含当前 make 会寻找的目录列表。你应当避免使用命令行参数 `-I` 来寻找以上这些默认目录，否则会使得 `make` “忘掉”所有已经设定的包含目录，包括默认目录。

如果文件查找过程中出现错误，会出现警告信息，或者致命错误。

使用下面命令：

```makefile
-include <filenames>...
```

表示，无论include过程中出现什么错误，都不要报错继续执行。

### 命令前加@

表示去除命令回响。

> It means "don't echo this command on the output."

### shell function,如 $(shell pwd)

$(shell command) 功能是做命令展开，它接受一个shell 命令作为一个参数，执行该shell 命令，并把shell 命令输出展开。类似于 ``。

> The `shell` function provides for `make` the same facility that backquotes provide in most shells: it does *command expansion*. This means that it takes as an argument a shell command and expands to the output of the command. The only processing `make` does on the result is to convert each newline (or carriage-return / newline pair) to a single space. If there is a trailing (carriage-return and) newline it will simply be removed.

### 条件语句

```makefile
libs_for_gcc = -lgnu
normal_libs =

foo: $(objects)
ifeq ($(CC),gcc)
        $(CC) -o foo $(objects) $(libs_for_gcc)
else
        $(CC) -o foo $(objects) $(normal_libs)
endif
```

`ifeq` 判断接着它后面的两个参数是否相等，相等就执行下面的语句，否则忽略它下面的语句。

`else` 当前面的`ifeq` 不成立，就会执行它下面的语句。else 语句是可选的。

`endif` 表示 ifeq 的结束。必须要有。

### Defining Variables Verbatim

define 指令可以定义带有换行符的变量，寻常定义方式做不到。

```makefile
define two-lines
echo foo
echo $(bar)
endef
```

two-lines 是变量名，两个 echo行 是变量的值。`endef` 表示该语句的结束，必须要有。

详细的看这里：

https://ftp.gnu.org/old-gnu/Manuals/make-3.80/html_node/make_71.html

### export 指令

顶层makefile 定义的变量可以向下传递给 子makefile。 使用 `export var...` 可以做到。

```makefile
# 将 CC CXX CFLAGS... 等变量传导到 子makefile (神奇的用法)
export CC CXX CFLAGS DRV_CFLAGS LDFLAGS OUTDIR TOPDIR DATEOBJ
```

详细的看官方文档：

https://www.gnu.org/software/make/manual/html_node/Variables_002fRecursion.html

### makefile 里面指令部分实际上是shell 命令

这是真的，真是神奇的理解。

下面是Makefile 的内容：

```makefile
hi = ls $(1)
all:
    $(call hi,~)
```

在终端执行：

```shell
make
```

结果就是 执行命令`ls ~` 的输出。
