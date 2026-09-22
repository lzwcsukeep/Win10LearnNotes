- `dpkg`（**Debian Package**）是 Debian、Ubuntu 等 Linux 发行版最底层的软件包管理工具，用来安装、卸载、查询和管理 `.deb` 软件包。

- .deb 包是一个软件安装包，包含二进制程序，文档，包信息等多个文件。
- dpkg 操作本地已经存在的 .deb 包，不会去网络上下载东西，或者解决依赖
- 和 apt 的区别： apt 会调用 dpkg 来安装包， apt 会自动去软件仓库下载包，下载依赖。
- 相当于 apt 是前端工具，dpkg 是后端工具。

常用 dpkg 选项：

```bash
 - Install a package:
   sudo dpkg {{[-i|--install]}} {{path/to/file.deb}}

 - Remove a package:
   sudo dpkg {{[-r|--remove]}} {{package}}

 - List installed packages:
   dpkg {{[-l|--list]}} {{pattern}}

 - List a package's contents:
   dpkg {{[-L|--listfiles]}} {{package}}

 - List contents of a local package file:
   dpkg {{[-c|--contents]}} {{path/to/file.deb}}

 - Find out which package owns a file:
   dpkg {{[-S|--search]}} {{path/to/file}}

 - Purge an installed or already removed package, including configuration:
   sudo dpkg {{[-P|--purge]}} {{package}}

```



