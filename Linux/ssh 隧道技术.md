机器网络背景：

主机 A ,中间机器 B（173.1.15.17） , 要访问的机器C(192.168.4.165)。 A 因为网络隔离无法访问 C ，但是 A 可以访问B, B 可以访问C。

由此可建立 ssh 隧道。

---

### 本地隧道

在 机器A 上运行 ：

```bash
ssh -L 2222:192.168.4.165:222 root@173.1.15.17
```

格式：

``` bash
ssh -L [本地端口]:[目标机器IP|被隔离的机器IP]:[目标机器的端口] [跳板机用户]@[跳板机IP]
即
在 A 上运行：
ssh -L [A端口]:[CIP][CPort] [BUser]@[BIP]
```

上述命令运行成功，则隧道建立。

之后 从 A 上 ssh 访问 C 的命令改为：

```bash
ssh -p [A端口] [CUser]@127.0.0.1
```

备注： 中间机器B 放在最后。 只需要一条命令就建立隧道。

#### 如果还要访问 C 上的网站

例如：

```
C:8080
```

可以建立：

```
ssh -L 8080:192.168.4.165:8080 root@173.1.15.17
```

格式：

```bash
ssh -L [LocalWebPort]:[CIP][CWebPort] [BUser]@[BIP]
```

然后浏览器访问：

```
http://127.0.0.1:[LocalWebPort]
```

实际上访问的是：

```
192.168.4.165:8080
// 即
C:Cport
```

### 动态隧道

格式

```bash
ssh -D 1080 root@173.1.15.17
```

或者

```bash
ssh -N -D 1080 root@173.1.15.17
```

其中：

- `-D 1080`

表示：

> 在本机监听 1080 端口，并提供 SOCKS5 服务。

- `-N`

表示：

> 不登录远程 shell，只建立隧道。

格式：

```bash
ssh -D [-N] [localPort] [remoteuser]@[remotehost]
```

动态隧道的含义就是在本地会开启一个监听的端口[localPort] ,然后本地所有往这个端口的流量都会被转发到远程机器[remotehost] ,远程机器会根据应用层协议自动寻找要连接的地方，然后从那个地方拿到数据，转发给本地端口。这个操作很神奇很牛逼。 直接把中间机器(remote host) 变成你所用了。

> from man ssh
>
> -D [bind_address:]port
>              Specifies a local “dynamic” application-level port forwarding.  This works by allocating a socket to listen to port on
>              the local side, optionally bound to the specified bind_address.  Whenever a connection is made to this port, the con‐
>              nection is forwarded over the secure channel, and the application protocol is then used to determine where to connect
>              to from the remote machine.  Currently the SOCKS4 and SOCKS5 protocols are supported, and ssh will act as a SOCKS
>              server.  Only root can forward privileged ports.  Dynamic port forwardings can also be specified in the configuration
>              file.

动态隧道相当于：

- **不需要提前指定目标地址和端口**；
- 可以访问跳板机能够访问的任意 IP 和端口；
- 本质上相当于在本机开了一个 SOCKS5 代理。

最关键的是浏览器也可以使用了！！！ 浏览器设置代理服务器那里选择 127.0.0.1，端口选 `ssh -D ...` 命令指定的端口[localPort] 