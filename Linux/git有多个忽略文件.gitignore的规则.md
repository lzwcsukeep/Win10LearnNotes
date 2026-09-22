**问 ： git 里面多个目录下有 .gitignore 是如何工作的?**

---

这是 Git 的一个重要机制。**Git 并不是只有仓库根目录的一个 `.gitignore` 生效，而是可以在任意目录放置 `.gitignore`，每个 `.gitignore` 只对它所在目录及其子目录生效。**

## Git 如何查找 `.gitignore`

假设你的仓库结构是：

```text
repo/
├── .gitignore
├── src/
│   ├── .gitignore
│   ├── backend/
│   │   ├── .gitignore
│   │   ├── bootstrap/
│   │   │   ├── .gitignore
│   │   │   └── bootparse.h
│   │   └── nodes/
│   │       ├── .gitignore
│   │       └── copyfuncs.funcs.c
│   └── include/
```

现在判断

```text
src/backend/bootstrap/bootparse.h
```

是否应该被忽略。

Git 会依次读取：

```text
repo/.gitignore
        ↓
repo/src/.gitignore
        ↓
repo/src/backend/.gitignore
        ↓
repo/src/backend/bootstrap/.gitignore
```

**路径上的所有 `.gitignore` 都会参与匹配。**

不会去读取：

```text
src/backend/nodes/.gitignore
```

因为它不是 `bootparse.h` 的祖先目录。

------

## 越靠近文件的 `.gitignore` 越方便维护

例如：

根目录：

```gitignore
*.o
```

表示整个项目：

> 所有 `.o` 文件都忽略。

------

而

```text
src/backend/bootstrap/.gitignore
```

里面：

```gitignore
/bootparse.h
```

这里只影响：

```text
src/backend/bootstrap/bootparse.h
```

不会影响：

```text
src/backend/parser/bootparse.h
```

因为它不是同一个目录。

------

## `/` 的含义

例如：

```gitignore
/bootparse.h
```

这里的 `/` **不是仓库根目录**。

它表示：

> **当前 `.gitignore` 所在目录。**

例如：

```
src/backend/bootstrap/.gitignore
```

里面：

```gitignore
/bootparse.h
```

等价于：

```
src/backend/bootstrap/bootparse.h
```

不会匹配：

```
src/backend/bootstrap/tmp/bootparse.h
```

如果写成：

```gitignore
bootparse.h
```

则会匹配：

```
bootstrap/bootparse.h
bootstrap/tmp/bootparse.h
bootstrap/a/b/bootparse.h
```

------

## PostgreSQL 为什么大量使用目录级 `.gitignore`

这是 PostgreSQL 源码的一个特点。

例如：

```
src/backend/bootstrap/
```

构建后只会生成：

```
bootparse.h
bootscanner.c
```

因此直接放：

```gitignore
/bootparse.h
/bootscanner.c
```

而

```
src/backend/nodes/
```

构建后生成的是：

```
copyfuncs.funcs.c
equalfuncs.funcs.c
outfuncs.funcs.c
```

所以这里单独维护：

```gitignore
*.funcs.c
node-support-stamp
```

这样每个模块只管理自己生成的文件，不需要在仓库根目录维护一个很长的 `.gitignore`。

------

## 如何查看是哪个 `.gitignore` 生效

你刚才执行的命令就是最好的方法：

```bash
git check-ignore -v src/backend/bootstrap/bootparse.h
```

输出：

```text
src/backend/bootstrap/.gitignore:1:/bootparse.h
```

意思就是：

- 生效文件：`src/backend/bootstrap/.gitignore`
- 第 1 行
- 规则：`/bootparse.h`

如果还有上层规则，也可能看到不同的匹配结果；`-v` 会告诉你最终命中的那条规则。

------

## 多个 `.gitignore` 会不会冲突？

会，但 Git 有明确的规则：**所有相关规则按顺序应用，后匹配到的规则可以覆盖前面的规则。**

例如：

根目录：

```gitignore
*.h
```

表示忽略所有头文件。

而：

```
src/backend/bootstrap/.gitignore
```

里面写：

```gitignore
!bootparse.h
```

这里的 `!` 表示取消忽略，那么：

```
src/backend/bootstrap/bootparse.h
```

最终就**不会**被忽略。

因此可以理解为：

1. 根目录规则提供全局默认行为；
2. 越靠近目标文件的 `.gitignore` 越适合定义局部规则；
3. 对于同一路径上的规则，后面的匹配可以覆盖前面的匹配（例如用 `!` 重新包含文件）。

这也是为什么像 PostgreSQL 这样的大型项目，会在很多子目录放置各自的 `.gitignore`，让每个模块自行管理自己的构建产物，而不是把所有忽略规则都集中到仓库根目录。