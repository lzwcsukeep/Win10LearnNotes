### make 可以使用环境变量

make 的变量可以来自它运行环境里面的环境变量。make 可见的环境变量都会以相同的名字和值转变

> Variables in `make` can come from the environment in which `make` is run. Every environment variable that `make` sees when it starts up is transformed into a `make` variable with the same name and value.

详细的看官方文档描述：

https://www.gnu.org/software/make/manual/html_node/Environment.html

## make -C dir 选项含义

在含有父子makefile 的项目下，通常在父makefile 下面使用 `-C dir` 选项进入dir 目录之后执行对应的make 命令。

比如在父makefile 有命令 `make -C dir all` ，这个命令效果等于 `cd dir && make all`

此时目录 dir 里的makefile 应该含有一个target all 。

注： -C 选项会自动打印 "Entering Directory dir ..." ，"Leaving Directory dir ..."等提示。

**使用-C 选项有什么好处？**

- **组织更清晰：** 不同模块、目录的构建规则分开管理。

- **提高可维护性：** 各子目录有各自 Makefile，主 Makefile 只负责调度。

- **避免路径混乱：** 使用 `-C` 代替手动切换目录，避免 recipe 里每行都加路径前缀。

详细介绍看：

https://chatgpt.com/c/6879a611-22f4-800a-893b-ad05b67ea5dc


