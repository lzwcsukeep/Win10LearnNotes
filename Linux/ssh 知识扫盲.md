# 什么是 OpenSSH？

**OpenSSH 是 SSH（Secure Shell）协议最流行、最标准的开源实现。**

可以把它理解成：

> **一套实现 SSH 协议的软件。**

就像：

- HTTP 是一种协议，而 **Nginx**、**Apache** 是 HTTP 协议的实现。
- Git 是一种版本控制工具，而 GitHub、GitLab 是基于 Git 的服务。
- SSH 是一种协议，而 **OpenSSH** 是实现 SSH 协议的软件。

# SSH 是协议，OpenSSH 是软件

例如：

```
SSH
```

只是一个通信规范。

规定：

- 如何认证用户
- 如何加密数据
- 如何建立连接
- 如何传输文件
- 如何建立隧道（Port Forwarding）

真正负责完成这些工作的，就是 OpenSSH。

------

# OpenSSH 包含哪些程序？

安装 OpenSSH 后，会得到很多命令。

最常见的是：

| 程序         | 作用             |
| ------------ | ---------------- |
| `ssh`        | 登录远程主机     |
| `sshd`       | SSH 服务端       |
| `scp`        | 安全复制文件     |
| `sftp`       | 安全文件传输     |
| `ssh-keygen` | 生成 SSH 密钥    |
| `ssh-agent`  | 管理私钥         |
| `ssh-add`    | 把私钥加入 Agent |

## ssh

例如：

```
ssh unvdb@192.168.4.165
```

就是 OpenSSH 提供的客户端。

------

## sshd

Linux 上：

```
systemctl status sshd
```

看到的：

```
sshd
```

就是：

> SSH Daemon（SSH 服务）

它一直监听：

```
22
```

或者：

```
222
```

等待别人连接。

# 文件 `C:\Users\unvdb\.ssh\config` 

`C:\Users\unvdb\.ssh\config` 这个文件可以说是 **Windows 上所有基于 OpenSSH 的 SSH 客户端的统一配置文件**。VS Code Remote-SSH、命令行 `ssh`、`scp`、`sftp`、Git（配置为 OpenSSH 时）都可以使用它。

它的位置：

```
C:\Users\unvdb\.ssh\config
```

作用相当于 Linux 上的：

```
~/.ssh/config
```

# 一、没有 config 文件时

假设你想登录 C：

```
Windows
    │
    ▼
B（173.1.11.14）
    │
    ▼
C（192.168.4.165:222）
```

每次都要输入完整命令：

```
ssh -J root@173.1.11.14 -p 222 unvdb@192.168.4.165
```

如果使用 Git：

```
git clone ssh://git@192.168.4.165:222/project.git
```

每个工具都要知道：

- 用户名
- IP
- 端口
- 跳板机

比较麻烦。

------

# 二、有 config 文件后

例如：

```
Host B
    HostName 173.1.11.14
    User root

Host pgdev
    HostName 192.168.4.165
    User unvdb
    Port 222
    ProxyJump B
```

以后所有程序都只需要：

```
ssh pgdev
```

OpenSSH 会自动展开成：

```
ssh \
    -J root@173.1.11.14 \
    -p 222 \
    unvdb@192.168.4.165
```

参考链接： https://chatgpt.com/c/6a45ca81-34e8-83ea-99cc-914e67716c0e