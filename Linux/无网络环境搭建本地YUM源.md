Q:如果我一台Centos 机器不能联网，但是可以传文件进去。这个场景可以用yum解决？

A:可以用 **YUM / DNF**，但**不能直接用联网的公共源**，必须改成**本地源 / 离线源**。

简单直接结论：
> 不能联网 = 不能用外网 yum 源
> 但你可以：**把 rpm 包传到机器上，搭一个本地 yum 源**，照样用 `yum install` 自动解决依赖。

下面给你说清楚怎么做、有几种方案。

---

# 1. 能不能用 yum？
**能，但必须是“本地 yum 源”**。

正常 yum 是这样：
`yum install nginx` → 去 CentOS 官网下载 rpm → 安装

离线机器是这样：
你把一堆 `.rpm` 传到服务器某个目录 → 告诉 yum “软件包就在这里” →
`yum install nginx` → 自动从本地目录找包、自动解决依赖。

---

# 2. 最常用、最简单的离线方案：本地目录源
## 步骤极简版
1. 在**能联网的同版本 CentOS 机器**上，把需要的包全下载下来
   ```bash
   yum install --downloadonly --downloaddir=/tmp/myrpms nginx
   ```
   它会把 nginx + 所有依赖都下载到 `/tmp/myrpms`

2. 把整个 `myrpms` 目录传到离线机器，比如 `/opt/local-yum`

3. 在离线机器上创建 repo 数据
   ```bash
   createrepo /opt/local-yum
   ```

4. 写一个本地 repo 配置
   ```bash
   vi /etc/yum.repos.d/local.repo
   ```
   内容：
   ```ini
   [local]
   name=Local Repository
   baseurl=file:///opt/local-yum
   enabled=1
   gpgcheck=0
   ```

5. 然后就可以正常用 yum 了
   ```bash
   yum install nginx
   ```
   ✅ 自动解决依赖，完全离线。
