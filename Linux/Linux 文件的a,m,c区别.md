## 一、a / m / c 的区别（访问哪种时间戳）

Linux 文件有三种时间戳，`find` 用首字母区分：

| 字母  | 名称                | 含义             | 何时更新                                                     |
| :---- | :------------------ | :--------------- | :----------------------------------------------------------- |
| **a** | access time (atime) | **访问时间**     | 文件被读取时（如 `cat`、`less`）                             |
| **m** | modify time (mtime) | **修改时间**     | 文件**内容**被改变时（如 `echo >> file`）                    |
| **c** | change time (ctime) | **状态改变时间** | 文件的 **inode 元数据**改变时（内容、权限、所有者、链接数等） |

### 关键点

- **mtime**：只关心"内容"是否变了。
- **ctime**：只要 inode 里任何信息变了就更新，包括内容变化（所以 mtime 变时 ctime 一定也变），也包括 `chmod`、`chown`、`mv` 等。
- **atime**：只关心"被读取"。

### 举例

bash

```
chmod 644 file    # ctime 变，mtime/atime 不变
echo hi >> file   # mtime 和 ctime 都变，atime 不变
cat file          # atime 变
```



> 注意：`c` 是 **change time**（状态改变），不是 creation time（创建时间）。Linux 传统上不保存创建时间