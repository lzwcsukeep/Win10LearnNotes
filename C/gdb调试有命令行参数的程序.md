## GDB  **调试“未运行”的程序 + 带命令行参数**

场景：

比如 我要调试 initdb 程序，但是 initdb 需要带 -D 参数运行，如右所示： `initdb -D PGDATA`

------

# 🔹 一、最常用方法（推荐）✅

```bash
gdb program
```

进入 gdb 后再传参数：

```bash
(gdb) run arg1 arg2 arg3
```

------

## ✅ 示例

```bash
gdb ./a.out
(gdb) run input.txt 123
```

👉 等价于：

```bash
./a.out input.txt 123
```

------

# 🔹 二、提前设置参数（更规范）

```bash
(gdb) set args arg1 arg2 arg3
(gdb) run
```

------

## ✅ 示例

```bash
gdb ./a.out
(gdb) set args input.txt 123
(gdb) run
```

------

## 📌 好处

- 参数只需设置一次
- 可以反复 `run` 不用重复输入

------

# 🔹 三、启动 gdb 时直接带参数

```bash
gdb --args program arg1 arg2 arg3
```

------

## ✅ 示例

```bash
gdb --args ./a.out input.txt 123
```

然后：

```bash
(gdb) run
```

------

## 📌 适合场景

- 参数很多
- 想一开始就固定好

------

# 🔹 四、三种方式对比

| 方法       | 写法       | 推荐程度 |
| ---------- | ---------- | -------- |
| 运行时传参 | `run args` | ⭐⭐⭐⭐     |
| 预设参数   | `set args` | ⭐⭐⭐⭐⭐    |
| 启动时传参 | `--args`   | ⭐⭐⭐⭐     |

------

# 🔹 五、一个容易踩坑的点 ⚠️

### ❗ 不要这样写：

```bash
gdb ./a.out input.txt
```

👉 这会被解释为：

```bash
gdb program core
```

也就是：

- `input.txt` 会被当成 **core 文件**
- ❌ 不是参数！

------

# 🔥 六、完整调试流程（实战）

```bash
gdb ./a.out
(gdb) break main
(gdb) set args input.txt 123
(gdb) run
(gdb) next
(gdb) print x
```

------

# 🚀 一句话总结

> 👉 **参数要么用 `run` 传，要么用 `set args`，不要直接跟在 gdb 后面**

------

如果你接下来要调试像 PostgreSQL 这种复杂程序，我可以再教你：

- 如何调试带环境变量的程序
- 如何模拟 stdin 输入
- 如何调试 daemon / 后台进程

这些都是进阶 gdb 必备技能。