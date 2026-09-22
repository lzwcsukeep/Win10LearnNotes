# 函数

- **filter函数**

语法：

```makefile
$(filter PATTERN..., TEXT)
```

功能：

过滤掉 `TEXT` 中所有不符合 `PATTERN` 的单词，**保留**符合模式的单词。

filter函数有两个参数，<pattern> 传入的是过滤的模式，<text> 传入的是源字符串，函数过滤出text中符合pattern模式（可以有多个pattern）的字符串。函数返回值为过滤出的字符串。

**示例：**

```
SRC_FILE = main.c led.c readme.txt start.S
OBJ_FILE = $(filter %.c %.S, $(SRC_FILE))
all:
    @echo $(OBJ_FILE)
```

从SRC_FILE变量中过滤出所有的.c文件和所有的.S文件，过滤的结果赋值给变量OBJ_FILE。SRC_FILE变量中.c文件有main.c 、led.c，.S文件有start.S，所以使用filter函数过滤之后OBJ_FILE变量内容  **"main.c led.c start.S"**

注：还有个和filter函数功能相反的函数——filter-out，两个函数参数一致，filter-out是过滤掉<text>中符合<pattern>的内容，返回剩下的内容。如：上例中filter换成filter-out，执行结果为“readme.txt”

- **if 函数**

语法：

```makefile
$(if CONDITION,THEN-PART)
$(if CONDITION,THEN-PART,ELSE-PART)
```

**CONDITION**：条件表达式。如果该展开值非空（即存在内容），则条件为**真**；如果展开值为空，则为**假**。

**THEN-PART**：条件为真时执行/返回的内容。

**ELSE-PART**：条件为假时执行/返回的内容（可选）

示例：

```makefile
DEBUG = true

# 如果 DEBUG 变量非空，则返回 "-g"，否则返回为空
CFLAGS = $(if $(DEBUG),-g) 

```

示例：

```makefile
MODE = release

# 如果 MODE 是 release，则优化；否则保留调试符号
BUILD_FLAG = $(if $(filter release,$(MODE)),-O2,-g)

```

