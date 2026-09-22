直接看 deepseek 回答。

# apt install 会走https 代理吗？

**会的，`apt install` 会遵循你配置的 HTTPS 代理设置。**

这取决于你是如何设置代理的，APT 主要通过以下两种方式读取代理配置：

### 方式一：APT 专用配置（最可靠）
如果你在 `/etc/apt/apt.conf.d/` 目录下的配置文件（如 `95proxies` 或 `proxy.conf`）中写入了配置，APT 会严格遵循：
```text
Acquire::https::Proxy "http://192.168.8.127:7890";
```
只要配置了 `Acquire::https::Proxy`，APT 就会使用该代理来处理 HTTPS 请求。

### 方式二：系统环境变量
APT 也支持读取系统的 `https_proxy` 环境变量。
但是，这里有一个**关键陷阱**：`sudo` 默认会重置环境变量。如果你只是在自己终端里 `export https_proxy=...`，然后用 `sudo apt install`，这个代理变量很可能被 sudo 丢弃，导致 APT 走直连。

### 针对你场景的建议
结合你之前的 `proxy_on` 函数（设置了 `http_proxy` 和 `https_proxy`），如果你希望 `sudo apt install` 稳定走代理，**最稳妥的做法是专门为 APT 创建一个配置文件**，而不是依赖环境变量：

```bash
# 创建 APT 专用代理配置文件
sudo tee /etc/apt/apt.conf.d/95proxies > /dev/null <<EOF
Acquire::http::Proxy "http://192.168.8.127:7890";
Acquire::https::Proxy "http://192.168.8.127:7890";
EOF
```
这样配置后，无论环境变量如何，`apt install` 都会强制走 HTTPS 代理。

注意： sudo 默认会重置环境变量

# 那如何取消 APT 走代理呢？

取消 APT 走代理，就是**删掉或注释掉你之前添加的 APT 专用代理配置**。分几种情况：

## 一、如果用的是 APT 专用配置文件（推荐清理方式）

先找到是哪个文件配的代理：

```bash
grep -r "Proxy" /etc/apt/apt.conf.d/
```

通常会看到类似 `/etc/apt/apt.conf.d/95proxies` 这种文件。然后：

```bash
# 直接删除
sudo rm /etc/apt/apt.conf.d/95proxies
```

或者不想删、只想临时禁用，就把它**注释掉**：

```bash
sudo nano /etc/apt/apt.conf.d/95proxies
```

把内容改成（行首加 `//` 或 `#`）：

```text
// Acquire::http::Proxy "http://192.168.8.127:7890";
// Acquire::https::Proxy "http://192.168.8.127:7890";
```

## 二、如果用的是环境变量

APT 会读 `http_proxy` / `https_proxy`。检查一下：

```bash
env | grep -i proxy
```

如果输出里有代理地址，根据它在哪设置来取消：

| 位置                     | 取消方法                                    |
| ------------------------ | ------------------------------------------- |
| 当前终端临时设置         | `unset http_proxy https_proxy`              |
| `~/.bashrc` / `~/.zshrc` | 删掉或注释掉那几行，然后 `source ~/.bashrc` |
| `/etc/environment`       | 删掉对应行（系统级，需 sudo）               |
| `/etc/profile.d/*.sh`    | 删掉对应文件或行                            |

> 你之前的 `proxy_on()` 函数只是**临时导出**，只要不执行 `proxy_on`、或执行 `unset http_proxy https_proxy`，就不会走代理。另外建议加一个对应的 `proxy_off`：

```bash
proxy_off()
{
    unset http_proxy https_proxy all_proxy
}
```

## 三、验证是否已取消

```bash
# 看 APT 配置里还有没有 Proxy
grep -r "Proxy" /etc/apt/apt.conf.d/

# 看环境变量
env | grep -i proxy
```

两条命令都**没有输出**，就说明 APT 已经不走代理了。

## 四、只想临时不走代理（不改配置）

如果配置还在，但某次想临时绕过：

```bash
# 环境变量方式：临时清空
sudo env -u http_proxy -u https_proxy apt update
```

但注意：**APT 专用配置文件里的 `Acquire::https::Proxy` 优先级更高**，环境变量清空也不管用。这种情况下只能改配置文件，或在命令行覆盖：

```bash
sudo apt -o Acquire::https::Proxy="" update
```

## 总结

| 你的情况                             | 取消方法                                   |
| ------------------------------------ | ------------------------------------------ |
| 配了 `/etc/apt/apt.conf.d/95proxies` | 删掉该文件或注释内容                       |
| 用了环境变量                         | `unset` 或从 rc 文件删除                   |
| 想临时绕过                           | `sudo apt -o Acquire::https::Proxy="" ...` |

清理完跑一次 `sudo apt update`，能正常连上源就说明成功取消了。