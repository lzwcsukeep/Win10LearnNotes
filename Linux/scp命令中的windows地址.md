### scp 中 windows 系统的地址写法

Linux 中scp 命令是一个非常有用的命令。

当从Linux 的命令行中使用该命令，将Linux 的文件发送到windows 系统中时，windows 的地址应该如下写：

```t4
scp example.txt lzw@192.168.72.1:E:/Files/Win10LearnNotes/Linux/三剑客/example.txt
```

其中`lzw@192.168.72.1` 是用户名和IP, 后面`E:/Files/Win10LearnNotes/Linux/三剑客/example.txt` 是windows 系统上的地址。

在Linux 上写windows 地址可以使用'/' 或者'\\\' ，两种都是有效的。

几点提示：

1. scp 命令中的地址可以是windows， 参照上面的地址写就没问题。

2. 在 Linux 上执行scp命令将文件传送到windows 系统上时，windows 上需要开启sshd 服务。

3. windows IP 地址的确定：
   
   - 当Linux 是windows 上的内部虚拟机时，使用window的 VMware Network 或者 WLAN 的IPv4 地址都可以成功发送。没有区别。




