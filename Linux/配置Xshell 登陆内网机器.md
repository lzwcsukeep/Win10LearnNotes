# 配置 Xshell 登陆内网机器

背景目的：

> 我本地主机 windows ,IP 192.168.2.128。 有一台中间机器 B ，IP 173.1.11.14 ，另一台机器C 192.168.4.165。 目前的情况是 我的windows 机器 可以 ssh 登陆 机器 B, 机器B 可以访问机器C,但是 windows 不能访问机器C ,机器C 是隔离的。机器C 上部署了 gitlab 服务，有一些git 仓库，我如何使用 xshell 通过 机器B 可以在 xshell 远程登陆 C ？

---

# 方法

你的网络拓扑可以表示为：

```text
Windows (192.168.2.128)
        │
        │ SSH 可达
        ▼
B (173.1.11.14)
        │
        │ 内网可达
        ▼
C (192.168.4.165)
      GitLab + SSH
```

你的目标是：

> **在 Windows 上通过 B，使用 Xshell 登录 C。**

这个场景就是 **SSH Jump Host（跳板机）**，有几种方法。

------

# 方法一：Xshell 的 SSH Tunnel（推荐）

这是最简单的方法，不需要修改 B 或 C。

## 第一步：先建立到 B 的连接

新建 Session：

```
Host : 173.1.11.14
User : root
```

可以正常登录。

------

## 第二步：配置 SSH Tunnel

进入

```
Properties
    Tunnel
```

添加一个 Tunnel：

```
Type:
Local

Listen Host
127.0.0.1

Listen Port
2222

Destination Host
192.168.4.165

Destination Port
22
```

即

```
127.0.0.1:2222
        │
        ▼
B
        │
        ▼
192.168.4.165:22
```

保存。

连接到 B。

此时 Windows 上会出现

```
localhost:2222
```

实际上就是

```
192.168.4.165:22
```

------

## 第三步：再建立一个 Xshell Session

Host：

```
127.0.0.1
```

Port：

```
2222
```

用户名：

```
git
或者
root
或者
你的用户
```

实际上就是登录到了 C。

------

# 方法二：命令行 ssh（最简单）

Windows 自带 OpenSSH：

```bash
ssh -J root@173.1.11.14 root@192.168.4.165
```

其中

```
-J
```

就是 Jump Host。

效果：

```
Windows
    │
 ssh
    │
    ▼
173.1.11.14
    │
 ssh
    │
    ▼
192.168.4.165
```

------

------

上述两种方法是经过实践检验过的，一定可以成功的。下面的方法作为扩展备用，但是还没有实践检验过。

# 扩展备用方法：使用你之前建立的 SOCKS5（你之前问过）

....

....



你之前建立的是：

```bash
ssh -D 2223 root@173.1.11.14
```

这是

```
SOCKS5 Proxy
localhost:2223
```

然后让 Xshell 使用

```
SOCKS5
127.0.0.1
2223
```

去连接

```
192.168.4.165
```

也可以。

不过 SSH 登录的话，这种方式没有直接端口转发方便。

------

# 如果你还想访问 GitLab（HTTP）

例如

```
http://192.168.4.165
```

可以建立

```
Local Port Forward
```

例如：

```
127.0.0.1:8080
        │
        ▼
192.168.4.165:80
```

如果 GitLab 是 HTTPS：

```
127.0.0.1:8443
        │
        ▼
192.168.4.165:443
```

然后浏览器访问：

```
http://127.0.0.1:8080
```

或者

```
https://127.0.0.1:8443
```

即可访问 C 上的 GitLab。

------

## 如果你希望使用 Git（clone/pull/push）通过跳板机访问 C 上的 GitLab SSH 仓库

最方便的方式是使用 OpenSSH 的 `ProxyJump` 配置，例如在 Windows 的 `~/.ssh/config` 中配置：

```text
Host jump
    HostName 173.1.11.14
    User root

Host gitlab-c
    HostName 192.168.4.165
    User git
    ProxyJump jump
```

之后无论是：

```bash
ssh gitlab-c
```

还是 Git 使用 SSH 地址（例如 `git@gitlab-c:group/project.git`），都会自动经过 B 跳转到 C，无需每次手动建立隧道。这也是长期使用 GitLab 最省事、最稳定的方案。