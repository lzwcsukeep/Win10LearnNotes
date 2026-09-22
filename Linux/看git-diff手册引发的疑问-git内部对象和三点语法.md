下面是 git diff Man 手册部分内容

```c
GIT-DIFF(1)                                                                                         Git Manual                                                                                     
    GIT-DIFF(1)

NAME
       git-diff - Show changes between commits, commit and working tree, etc

SYNOPSIS
       git diff [<options>] [<commit>] [--] [<path>...]
       git diff [<options>] --cached [--merge-base] [<commit>] [--] [<path>...]
       git diff [<options>] [--merge-base] <commit> [<commit>...] <commit> [--] [<path>...]
       git diff [<options>] <commit>...<commit> [--] [<path>...]
       git diff [<options>] <blob> <blob>
       git diff [<options>] --no-index [--] <path> <path>
```

好奇的是 ：

git diff man 手册提到的 blob 的意思，顺便解释 --merge-base 的含义。解释 `git diff [<options>] <commit>...<commit> [--] [<path>...]` 含义

---

这几个概念是 `git diff` 手册中最容易让人困惑的部分，它们涉及 Git 的对象模型（Object Model）和提交图（Commit Graph）。下面分别解释。

------

# 一、什么是 blob？

Git 内部一共有四种对象（Object）：

| 类型   | 保存内容 |
| ------ | -------- |
| blob   | 文件内容 |
| tree   | 目录     |
| commit | 一次提交 |
| tag    | 标签     |

其中 **blob(Binary Large Object)** 就是**一个文件的内容**。

例如有一个文件

```
hello.txt

hello world
```

Git 保存以后会生成一个 blob：

```
blob
 └── "hello world\n"
```

blob **只保存内容**，并不知道：

- 文件叫什么
- 放在哪个目录
- 文件权限

这些信息都保存在 tree 中。

例如：

```
project
 ├── hello.txt
 └── readme.md
```

Git 内部其实是

```
commit
   │
   ▼
 tree
 ├── hello.txt -> blob A
 └── readme.md -> blob B
```

所以：

```
commit
   ↓
tree
   ↓
blob
```

------

## 查看 blob

例如

```
git ls-tree HEAD
```

输出

```
100644 blob e965047ad7c57865823c7d992b1d046ea66edf78    hello.txt
```

这里

```
e965047...
```

就是 blob id。

可以查看

```
git cat-file -p e965047ad7c57865823c7d992b1d046ea66edf78
```

输出

```
hello world
```

说明 blob 就是文件内容。

------

# git diff

这是 man 手册最后一行：

```
git diff <blob> <blob>
```

意思就是：

**直接比较两个文件内容对象。**

例如：

```
git diff \
e965047ad7 \
7f48392bd1
```

Git 根本不关心：

- commit
- branch
- tree

而是直接把两个 blob 当作两个文本比较。

这种命令比较少见，但在 Git 内部经常用到。

------

## blob 可以来自不同 commit

例如

```
HEAD
 hello.c -> blob A

HEAD~3
 hello.c -> blob B
```

可以

```
git diff blobA blobB
```

效果和

```
git diff HEAD~3 HEAD -- hello.c
```

差不多。

------

# 二、什么是 --merge-base？

这是 Git graph 中的重要概念。

假设：

```
  A
   \
    B
     \
      C
     /  \
    E     D  (master)
   /
  F (feature)
```

共同祖先（merge base）是

```
C
```

Git 可以计算：

```
git merge-base master feature
```

输出

```
C
```

------

## 为什么需要 merge-base？

如果直接

```
git diff master feature
```

实际上比较的是

```
D
vs
F
```

即：

```
master 的快照
vs
feature 的快照
```

很多时候，我们真正关心的是：

> feature 分支到底改了什么？

那么应该比较：

```
merge-base(master, feature)
        ↓
        C
        │
        ▼
        F
```

也就是

```
C
vs
F
```

这就是

```
git diff --merge-base master feature
```

等价于

```
git diff $(git merge-base master feature) feature
```

------

举例

```
master

A-B-C-D

feature

     \
      E-F
```

merge base

```
C
```

则

```
git diff master feature
```

比较

```
D
vs
F
```

而

```
git diff --merge-base master feature
```

比较

```
C
vs
F
```

后者通常就是：

> feature 分支新增了哪些修改。

---

备注： 下面这句话解释的很清楚

>  **merge base** is the **best common ancestor commit shared by two or more branches**. It represents the last point in time where the branches shared the exact same history before they diverged and went their separate ways.

**特殊的，当 master 本身就是 feature 的父commit时， `git merge-base master feature` 的结果就是 master 。**

一句话，merge-base 找两个分支最后的相同历史点。

# 三、git diff A...B 的含义

这是很多人容易误解的地方。

man 手册：

```
git diff <commit>...<commit>
```

三个点 **并不是**：

```
A
vs
B
```

而是：

```
merge-base(A,B)
      ↓
      B
```

也就是说

```
git diff A...B
```

实际上等价于

```
git diff $(git merge-base A B) B
```

注意：

**只取第二个 commit！**

这是和 `git log` 最大的区别。

------

例如：

```
master

A-B-C-D

feature

     \
      E-F
git diff master...feature
```

实际上比较的是

```
C
vs
F
```

而不是

```
D
vs
F
```

------

## 如果交换顺序

```
git diff feature...master
```

比较的是

```
C
vs
D
```

所以：

```
A...B
```

和

```
B...A
```

结果通常不同。

因为第二个参数不同。

------

# 四、为什么这样设计？

因为 Git 的典型开发流程就是：

```
master

A-B-C-D

feature

     \
      E-F
```

开发者想知道：

> feature 相对于 master 新增了哪些修改？

直接

```
git diff master feature
```

看到的是：

```
master 的所有变化
+
feature 的变化
```

这并不是想看的。

真正需要的是：

```
共同祖先
↓

feature
```

因此：

```
git diff master...feature
```

就成了最常用的写法。

------

# 五、与 git log 的区别

这里特别容易混淆：

```
git diff A...B
```

表示：

```
merge-base(A,B)
        ↓
        B
```

而

```
git log A...B
```

表示：

```
(A ∪ B)
-
(A ∩ B)
```

即**对称差集（symmetric difference）**：显示能从 `A` 或 `B` 到达、但不能同时从两者到达的提交。

因此：

- `git diff` 中的 `...`：表示“**从共同祖先到第二个提交的内容差异**”。
- `git log` 中的 `...`：表示“**两个分支各自独有的提交**”。

虽然语法相同，但语义完全不同。

------

## 总结

| 命令                        | 实际比较对象                                        |
| --------------------------- | --------------------------------------------------- |
| `git diff A B`              | `A` 的快照 vs `B` 的快照                            |
| `git diff A...B`            | `merge-base(A,B)` vs `B`                            |
| `git diff --merge-base A B` | 与 `git diff A...B` 等价（当只有两个提交参数时）    |
| `git diff <blob1> <blob2>`  | 两个 blob（文件内容对象）的差异，不经过 commit/tree |
| `git merge-base A B`        | 找到 `A` 与 `B` 最近的共同祖先提交                  |