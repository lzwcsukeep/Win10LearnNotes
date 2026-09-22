# `find` 中 a/m/c 与 time/min 的区别

## 一、a / m / c 的区别（访问哪种时间戳）

Linux 文件有三种时间戳，`find` 用首字母区分：

| 字母  | 名称                | 含义             | 何时更新                                                     |
| ----- | ------------------- | ---------------- | ------------------------------------------------------------ |
| **a** | access time (atime) | **访问时间**     | 文件被读取时（如 `cat`、`less`）                             |
| **m** | modify time (mtime) | **修改时间**     | 文件**内容**被改变时（如 `echo >> file`）                    |
| **c** | change time (ctime) | **状态改变时间** | 文件的 **inode 元数据**改变时（内容、权限、所有者、链接数等） |

### 关键点

- **mtime**：只关心"内容"是否变了。
- **ctime**：只要 inode 里任何信息变了就更新，包括内容变化（所以 mtime 变时 ctime 一定也变），也包括 `chmod`、`chown`、`mv` 等。
- **atime**：只关心"被读取"。

### 举例

```bash
chmod 644 file    # ctime 变，mtime/atime 不变
echo hi >> file   # mtime 和 ctime 都变，atime 不变
cat file          # atime 变
```

> 注意：`c` 是 **change time**（状态改变），不是 creation time（创建时间）。Linux 传统上不保存创建时间。

---

## 二、time / min 的区别（时间单位）

| 后缀     | 单位          | 说明                      |
| -------- | ------------- | ------------------------- |
| **min**  | 分钟          | 例如 `-mmin 5` = 5 分钟前 |
| **time** | 24 小时（天） | 例如 `-mtime 5` = 5 天前  |

所以：
- `-amin` / `-mmin` / `-cmin` → 以**分钟**为单位
- `-atime` / `-mtime` / `-ctime` → 以**24小时**为单位

---

## 三、`n` 的符号：+ / - / 无

这是最容易混淆的地方：

| 写法 | 含义                     |
| ---- | ------------------------ |
| `n`  | 恰好等于 n（向下取整后） |
| `+n` | **大于** n               |
| `-n` | **小于** n               |

### 重要：`-atime` 的取整规则（手册重点）

`find` 计算"多少天前"时会**忽略小数部分**（向下取整）。

```bash
find . -atime +1
```
含义是"访问时间在 **2 天前以上**"，而不是"超过 1 天"。

**原因**：文件在 1.9 天前访问，取整后 = 1，不满足 `+1`（要求 >1）。必须 ≥ 2 天才算 `+1`。

同理 `-atime 1` 表示取整后恰好等于 1，即访问时间在 [1天, 2天) 区间内。

`-mmin` / `-cmin` 也是同样的取整逻辑，只是单位换成分钟。

---

## 四、组合速查表

| 命令        | 含义                        |
| ----------- | --------------------------- |
| `-amin -10` | 10 分钟内被访问过           |
| `-atime +7` | 超过 7 天（即 ≥8 天）未访问 |
| `-mmin -30` | 30 分钟内内容被修改过       |
| `-mtime 1`  | 内容在 1~2 天前被修改       |
| `-cmin +60` | 状态改变超过 60 分钟前      |
| `-ctime -1` | 状态在 24 小时内改变过      |

---

## 五、一句话总结

> **a/m/c** 决定看哪个时间戳（访问 / 内容修改 / 状态改变），**time/min** 决定单位（天 / 分钟），**n 的正负号**决定是"大于"还是"小于"，且 `time` 系列会向下取整，`+1` 实际表示"2 天以上"。

### 常用实战

```bash
# 找出 7 天未修改的日志并删除（注意 +7 实际是 8 天以上）
find /var/log -name "*.log" -mtime +7 -delete

# 找出最近 10 分钟修改过的文件
find . -type f -mmin -10

# 找出权限刚被改过的文件（排查入侵）
find /etc -cmin -60
```

> from man find:
>
>        -amin n
>               File was last accessed less than, more than or exactly n minutes ago.
>         
>        -atime n
>               File  was last accessed less than, more than or exactly n*24 hours ago.  When find
>               figures out how many 24-hour periods ago the file was  last  accessed,  any  frac‐
>               tional part is ignored, so to match -atime +1, a file has to have been accessed at
>               least two days ago.
>         
>        -cmin n
>               File's status was last changed less than, more than or exactly n minutes ago.
>         
>        -ctime n
>               File's  status  was  last  changed less than, more than or exactly n*24 hours ago.
>               See the comments for -atime to understand how rounding affects the  interpretation
>               of file status change times.
>        -mmin n
>               File's data was last modified less than, more than or exactly n minutes ago.
>         
>        -mtime n
>               File's data was last modified less than, more than or exactly n*24 hours ago.  See
>               the  comments  for -atime to understand how rounding affects the interpretation of
>               file modification times.
>
> 命令解释 -amin n :  -选项开头， a=access,min=分钟， n=计量数，n 等于n, +n 大于n ,-n 小于n

