## 🧭 什么是 apt 的“源”？

在 Ubuntu（或 Debian 系列）中，`apt` 是用来安装和更新软件的工具，而“**源（Repository）**”就是 `apt` 获取软件包的**服务器地址**。

换句话说：

> apt 源 = 你系统软件包的“下载商店”

就像你手机的 App Store、华为应用市场、小米商店一样，Linux 系统的软件不是从网站下载 `.exe`，而是通过配置好的“源”自动安装和更新。

## 🧱 源的本质

在 `/etc/apt/sources.list` 和 `/etc/apt/sources.list.d/*.list` 文件中，列出了许多地址，像这样：

`deb http://mirrors.aliyun.com/ubuntu focal main restricted universe multiverse`

这一行可以解释成：

- `deb`：二进制包（还有 `deb-src` 是源代码包）

- `http://mirrors.aliyun.com/ubuntu`：软件源的服务器地址

- `focal`：表示 Ubuntu 20.04 的代号（不同版本代号不同）

- `main restricted universe multiverse`：软件分类（是否开源、是否受支持等）

## 🛒 这些源里都有什么？

每个源仓库里都包含了大量 `.deb` 包（就是软件包），以及元数据（版本号、依赖信息等）。

举例：

- `apt install samba` 会从源里找到 `samba` 这个软件包，下载并安装；

- `apt update` 会从源中**同步元数据**，知道“有哪些软件”、“哪些有更新”；

- `apt upgrade` 会实际下载更新包并安装。

## 📁 源配置文件存放在哪？

1. 主配置文件：
   
   bash
   
   复制编辑
   
   `/etc/apt/sources.list`

2. 附加源目录（如 PPA）：
   
   bash
   
   复制编辑
   
   `/etc/apt/sources.list.d/`

### 总结：

`源`的概念在Ubuntu 中实际就是下载软件包的服务器地址。有命令可以增加源，删除源来管理源。源有官方源，第三方源。增加第三方源就是增加一个下载地址。


