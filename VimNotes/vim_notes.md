# vim 小知识

## operator + motion

vim 中有motion,operator。 一个 operator 后面加motion表示对这个移动范围内的字符进行operator 操作。
这种情况下，移动开始和结束的字符包不包括在operator 内是有区别的。
这种区别分为 includsive 和 excludsive 。
includesive ：移动的最后一个字符也包括在内。
excludesive : 移动的最后一个字符不包括在内。

光标开始位置处的字符始终包含在内。

## path 中 "." 和 ",," 的区别

vim 中 在设置选项 'path' 时有两种相对路径。

1. 添加当前目录为搜索路径,使用 `:path+=,,` 即两个逗号。注： vim 有当前目录的概念，使用`:pwd` 可查看
2. 添加当前打开文件所在目录为搜索路径为: `:path+=.`
注意区分这两个的不同： ",," 表示添加当前 vim 的工作目录为搜索路径， "." 表示添加vim当前打开的文件所在的目录。这两个是有区别的。

**即：**

"." 是添加当前打开文件的所在目录为搜索目录，
",," 是将 vim 当前的工作目录添加为搜索目录

## 选项 tags 和 path 的区别

set tags= 或者 set path= 后面都是跟着字符串，且都是用逗号分隔，但是这两个选项是有区别的。

1. tags 选项后面是文件名的列表，而 path 后面是目录的列表。
2. tags 选项是给tags 命令用来查找 tags 文件用的，tags 选项指明的是文件名列表。tags 会在这些文件中查找某个tag.
而 path 选项是给gf,:find,:sfind 等命令用来在目录列表中查找某个文件用的。path 选项指明的是待查找的目录列表，
gf等命令会在这些目录中查找某个文件。

相同点： 这两个选项中都可以使用通配符"*"或者"**"

## 关于tags 选项的细节

来自vim 官方文档：

When a tag file name starts with "./", the '.' is replaced with the path of
the current file.  This makes it possible to use a tags file in the directory
where the current file is (no matter what the current directory is).  The idea
of using "./" is that you can define which tag file is searched first: In the
current directory ("tags,./tags") or in the directory of the current file
("./tags,tags").

For example:
        :set tags=./tags,tags,/home/user/commontags

In this example the tag will first be searched for in the file "tags" in the
directory where the current file is.  Next the "tags" file in the current
directory.  If it is not found there, then the file "/home/user/commontags"
will be searched for the tag.

---
和 path 选项类似的细节，注意区分 当前文件所在目录和 vim 当前工作目录。

./tags : 表示当前打开文件所在目录下的文件名为 tags 的文件。

tags : vim 当前工作目录下的文件名为 tags 文件。

比如当前打开文件为/pro/code/src/main.cpp ，而工作目录是/pro，那么 ./tags 表示/pro/code/src/tags
而tags 表示 /pro/tags.

## vim word motion 的通俗理解

b : to begin of current word
w : to start of next word
e : to end of current word

## 对文件进行加密

1. 开始编辑一个加密文件，使用 `-x` 参数启动vim, 如： vim -x en_file
2. 在已打开 vim 编辑一个文件时使用 `:X` 命令加密保存当前编辑文件。
3. 在 vim 正在编辑文件时，使用命令 `:set key=` 不再加密当前文件。

使用 vim 加密的文件，提供了错误的密码会是乱码，以及用其他命令查看会是乱码，
比如 cat en_file 或者 sed '' en_file 都是乱码。
