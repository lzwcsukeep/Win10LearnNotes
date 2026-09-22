# git ls-tree 用法

git ls-tree 是 git 里面比较内部高级的用法，涉及 git 的内部对象模型。首先 git 内部的对象类型有：

blob, tree , commit 。



一个 blob 表示的是一个磁盘上的文件。 我们可以用 git cat-file <blob> 查看这个 git blob 的内容，就像在 Bash 使用 cat file 一样查看一个文件的内容一样。

一个 tree 表示的是一个目录。目录就有目录条目，而且目录条目既可以是文件，也可以是子目录。 git ls-tree 就是用来查看这个 tree目录有哪些条目的，

就类似 Bash 里面 ls 命令一样。



因此 git ls-tree 的作用就是 查看 git 一个 tree object 里面有哪些条目。

git ls-tree 语法格式：

```bash
NAME
       git-ls-tree - List the contents of a tree object

SYNOPSIS
       git ls-tree [-d] [-r] [-t] [-l] [-z]
                   [--name-only] [--name-status] [--full-name] [--full-tree] [--abbrev[=<n>]]
                   <tree-ish> [<path>...]
```

比如：查看 HEAD 确定的 tree 的条目

```bash
[unvdb@165--165:~/workspace/repo_forks/UDB-TX]$ git ls-tree HEAD
100755 blob 8ea865e71b568e04d959cf8fe32ee5f839372336    .abi-compliance-history
100755 blob a1093f11dcaa5337502da7287ff3e26fd4d9eec4    .cirrus.star
100755 blob 8f5360cec9a252bef8f806b72a90295574343c66    .cirrus.tasks.yml
100755 blob f270f61241f81d5ef4fe39145e09bdf6aa505efe    .cirrus.yml
100755 blob a13b1e5519aaff16bb7332611b372a98964d415c    .git-blame-ignore-revs
100755 blob 47967c038902e177f033b4010071b67e72dcff3f    .gitattributes
100755 blob b09fb823c32129b6529505053d0e769f02ec94df    .gitignore
100755 blob f4c86a1b6a565613972d5c0e8de8c63d01740804    .gitlab-ci.yml
100644 blob 3f5a2f54f6e896a2f3fd79845a8acfa81140ef7e    .gitmodules
040000 tree cdef67cca7153d421f2826c9801376657125c03d    .vscode
100644 blob 3b79b1b4ca0dbe0600e23f3b23fe5bffdd8675ac    COPYRIGHT
100755 blob eab08b7b9ee0e18e1f176733ae2f0e5d19be0d37    GNUmakefile.in
100755 blob e7b168ae4f191cd8ffc86027285c440c4d06a94f    HISTORY
100755 blob 0155282c8a71ca56c793fc4025964ff7c57c32e9    Makefile
100755 blob f0d0510c19816fa6f3494fee3e5f57f0ee09460b    README
100755 blob 3b24638008a7fa1ab4f9f66790662e1dd1ca8425    README.md
100755 blob d05f8f9e3a80a38218f4e16e3dfe11037a34d0f9    aclocal.m4
040000 tree 6d5f47f0650aea011f5a17cd867d64d1888de5cf    cicd
040000 tree 0b14ae5ec30591a60acea9648f1704f006e3abb5    config
100755 blob 481ddd31848a08de7ec12ff234243d339da35874    configure
100755 blob ae54fa3a044a10a529e8d5c1f0b35f73709b3a85    configure.ac
040000 tree 52c1713446a77447a840bfa850312bbd9ab467af    contrib
040000 tree ee2f2d00bc3c21f51483e64c7d73dbeef6da9348    doc
100755 blob 3a9a806278e598d216d29d5845685e2c89694c9c    meson.build
100755 blob e0805896f5e1c951c84c7d4c1fe3b2f5c6b790ca    meson_options.txt
040000 tree ebaeb7791f8468369950b0609eec6eb6ad91f12e    pkg
040000 tree e418c204a9949a3491398bc33003de21b395c1a6    src
```

备注： git ls-tree 就是用来查看 tree object 的条目的，但是 后面也可以跟 commit 对象，因为一个 commit 对应一个 tree ，当后面跟 commit 时，会先确定 这个 commit 对应的 tree , 然后列举 这个 tree 的条目。

**git ls-tree <tree-ish>  path...**

git ls-tree 在 tree-ish 后面可以指定 path , 这种格式的含义就是 当前命令只关心 tree 下面的 path 条目，只需要列出 tree 下面 path 指定的条目就行。其他都不需要列出。

比如：只列出 HEAD 对应的 tree 下面 src 条目

```bash
[unvdb@165--165:~/workspace/repo_forks/UDB-TX]$ git ls-tree HEAD src
040000 tree e418c204a9949a3491398bc33003de21b395c1a6    src
```

注意上面就只列举出了 src 这个条目。



总结： git ls-tree 命令类似于 bash 上的 ls 命令，用来列举 git 内部对象 tree 的条目。当加了 path 后，只会列举 path 条目。 git ls-tree 后面跟的对象不仅仅是 tree, 其他可以确定 tree 的对象也可以，比如 commit 。git 官方把这种对象叫做 tree-ish。