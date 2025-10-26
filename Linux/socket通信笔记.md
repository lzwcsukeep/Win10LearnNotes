## 概念

socket 通信表示两个端点之间的通信，英文叫做endpoints。 这两个端点通信是个比较泛的概念，还可以继续细分，有domain, type, protocol。

### domain

这里可以选择是IPv4 还是 IPv6 还是 unix本机通信。

关于domain描述的英文原文：

> The domain argument determines the nature of the communication, including **the address format** (described in more detail in the next section). Figure 16.1 summarizes the domains specified by POSIX.1. The constants start with AF_ (for address family) because each domain has its own format for representing an address.

domain 确定了通信的本质属性，包括地址格式。AF_ 是address family 的缩写。

POSIX.1 规定的domains:

- Domain                         Description:

- AF_INET                         IPv4 Internet domain

- AF_INET6                       IPv6 Internet domain (optional in POSIX.1)

- AF_UNIX                        UNIX domain

- AF_UNSPEC                  unspecified

### type

这里可以选择UDP 还是 TCP 。

> The type argument determines the type of the socket, which further determines the communication characteristics. The socket types defined by POSIX.1 are summarized in Figure 16.2, but implementations are free to add support for additional types.

**Socket types：**

- SOCK_DGRAM ： fixed-length, connectionless, unreliable messages

- SOCK_RAW ： datagram interface to IP (optional in POSIX.1)

- SOCK_SEQPACKET ：fixed-length, sequenced, reliable, connection-oriented messages

- SOCK_STREAM : sequenced, reliable, bidirectional, connection-oriented byte streams

### protocol

The protocol argument is usually zero, to select the default protocol for the given
domain and socket type. When multiple protocols are supported for the same domain
and socket type, we can use the protocol argument to select a particular protocol. The
default protocol for a SOCK_STREAM socket in the AF_INET communication domain is

TCP (Transmission Control Protocol). The default protocol for a SOCK_DGRAM socket in
the AF_INET communication domain is UDP (User Datagram Protocol).

注： 这个参数一般是0(表示选择默认协议)，但是其真实含义是domain和 type 共同确定的协议有多个时，根据protocol 来确定使用哪个。AF_INET + SOCK_STREAM 的默认是TCP,

AF_INET + SOCK_DGRAM 的默认是UDP 。

### socket() 函数创建一个通信用的sockfd

```c
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
Returns: file (socket) descriptor if OK, −1 on error
```

socket() 函数三个参数的含义如上所讲。返回一个文件描述符。

## 通信地址的表示

> An address identifies a socket endpoint in a particular communication domain. The address format is specific to the particular domain. So that addresses with different formats can be passed to the socket functions, the addresses are cast to a generic sockaddr address structure:

通用的sockaddr structure:

```c
// socket 通信，表示地址最通用的结构体
struct sockaddr {
sa_family_t sa_family;/* address family */
char sa_data[];/* variable-length address */
.
.
.
};
```

### IPv4 地址表示

Internet addresses are defined in <netinet/in.h>. In the IPv4 Internet domain
(AF_INET), a socket address is represented by a **sockaddr_in** structure:

```c
// 仅表示IPv4 主机网络地址
struct in_addr {
in_addr_t s_addr;/* IPv4 address */
};

// 表示一个IPv4 socket 端end
struct sockaddr_in {
sa_family_t sin_family; /* address family */
in_port_t sin_port; /* port number */
struct in_addr sin_addr;/* IPv4 address */
};
```

### IPv6 地址表示

In contrast to the AF_INET domain, the IPv6 Internet domain (AF_INET6) socket
address is represented by a **sockaddr_in6** structure:

```c
struct in6_addr {
uint8_t s6_addr[16];/* IPv6 address */
};

struct sockaddr_in6 {
sa_family_t sin6_family;/* address family */
in_port_t sin6_port;/* port number */
uint32_t sin6_flowinfo;/* traffic class and flow info */
struct in6_addr sin6_addr;/* IPv6 address */
uint32_t sin6_scope_id;/* set of interfaces for scope */
};
```

The structure of a UNIX domain socket address is differentfrom both of the Internet domain socket address formats.

## 打印人类可读的地址

> It is sometimes necessary to print an address in a format that is understandable by a person instead of a computer. The BSD networking software included the `inet_addr` and `inet_ntoa`functions to convert between the binary address format and a string in dotted-decimal notation (a.b.c.d).These functions, however, work only with IPv4 addresses. Two new functions—`inet_ntop` and `inet_pton` —support similar functionality and work with both IPv4 and IPv6 addresses.

只关注通用的函数inet_pton 、inet_ntop

```c
#include <arpa/inet.h>

// 将字符串地址转换为二进制地址（presentation → numeric）
int inet_pton(int domain,        // 地址族（AF_INET 表示 IPv4，AF_INET6 表示 IPv6）
              const char *str,   // 输入：人类可读的地址字符串（如 "192.168.1.1"）
              void *addr);       // 输出：存放转换后的二进制地址的缓冲区
// 返回值：1 成功；0 地址格式无效；-1 错误（如 domain 不支持）

// 将二进制地址转换为字符串地址（numeric → presentation）
const char *inet_ntop(int domain,       // 地址族（AF_INET 或 AF_INET6）
                      const void *addr, // 输入：二进制地址
                      char *str,        // 输出：存放转换后字符串的缓冲区
                      socklen_t size);  // 缓冲区大小（需足够容纳字符串，如 IPv4 至少 16 字节）
// 返回值：成功返回字符串指针；失败返回 NULL
```

IPv4 地址字符串表示和二进制表示分析：

> //192.168.1.100 一个IPv4 地址由四个整数组成，一个整数在0-255之间，用一个字节就可以表示
> 
> //一个IPv4 地址，有四个整数，只需要四个字节就可以表示。而如果是字符串就需要很长的字节表示了。
> 
> // 比如192.168.1.100 含有13个字符。需要13个字节表示。而其实二进制无论如何都只需要4字节。
> 
> // 4字节，刚好32位无符号整数可以保存。
> 
> // 因此 IPv4 地址之间的转化 其实就是 IPv4地址字符串表示 <--> IPv4地址32位无符号整数 之间的转换。
> 
> // 也是 inet_pton() , inet_ntop() 函数的功能。

### 根据sockfd 描述符获取自身地址或对端地址

有两个系统调用可以根据 socket 通信的fd获取自身或者对端的地址。分别是：

getpeername, getsockname

原型：

```c
int getpeername(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

> DESCRIPTION
>        getpeername()  returns the address of the peer connected to the socket sockfd, in the buffer pointed to by addr.  The addrlen argument should be initialized to
>        indicate the amount of space pointed to by addr.  On return it contains the actual size of the name returned (in bytes).  The name is truncated if  the  buffer provided is too small.

原型：

```c
#include <sys/socket.h>
int getsockname(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

> DESCRIPTION
>        getsockname() returns the current address to which the socket sockfd is bound, in the buffer pointed to by addr.  The addrlen argument should be initialized to
>        indicate the amount of space (in bytes) pointed to by addr.  On return it contains the actual size of the socket address.
> The returned address is truncated if the buffer provided is too small; in this case, addrlen will return a value greater than was supplied to the call.



注意函数名字都说是获取name, name 这个名字有误导性。实际上获取的是sockfd 对应的

struct sockaddr 结构体。存放在指针参数 addr所指位置。

为了获得人类可读的IP和端口，一般还需要利用inet_ntop 和 ntohs 函数。

```c
    inet_ntop(AF_INET, &addr.sin_addr, ip_str, INET_ADDRSTRLEN);
    printf("%s 地址：%s:%d\n", desc, ip_str, ntohs(addr.sin_port));
```

示例函数如下：

```c
// 打印自身地址
// getsockname 换成getpeername 一样的用法，只是获取了sockaddr 结构体
// 还需要inet_ntop 和 ntohs 转成人类可读的形式，IP和端口
void print_socket_addr(int sockfd, const char *desc) {
    struct sockaddr_in addr;
    socklen_t addr_len = sizeof(addr);
    char ip_str[INET_ADDRSTRLEN];
    if (getsockname(sockfd, (struct sockaddr *)&addr, &addr_len) == -1) {
        perror("getsockname 失败");
        return;
    }
    inet_ntop(AF_INET, &addr.sin_addr, ip_str, INET_ADDRSTRLEN);
    printf("%s 地址：%s:%d\n", desc, ip_str, ntohs(addr.sin_port));
}
```

重点： getpeername, getsockname 都是根据fd获取 struct sockaddr 结构体。函数名里面的name 有误导性。


