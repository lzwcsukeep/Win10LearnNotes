 非常有用的Linux命令 `scp`

scp 命令即可以从本地机器传送文件到远端机器，也可以反方向从远端机器拷贝文件到本地机器，还可以从一台远端机器拷贝文件到另一台远端机器。

scp 的命令格式：

```shortcode
scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2
```

第一个参数是源地址，第二个参数是目标地址。

scp根据地址有没有冒号`:`来区分本地机器还是远端机器。因此两个地址都有冒号（隔开用户名和IP地址）那就是从一台远端机器拷贝文件到另一台远端机器。如果只有一个地址有冒号，另一个地址是相对地址或绝对地址，那么就是本地机器和远端机器之间传输文件，传输的方向从第一个参数(SRC_HOST)到第二个参数(DEST_HOST).

英文:

> The `scp` command syntax take the following form:
> 
> ```shell
> scp [OPTION] [user@]SRC_HOST:]file1 [user@]DEST_HOST:]file2
> ```
> 
> Copy
> 
> - `OPTION` - [scp options](https://linux.die.net/man/1/scp) such as cipher, ssh configuration, ssh port, limit, recursive copy …etc.
> - `[user@]SRC_HOST:]file1` - Source file.
> - `[user@]DEST_HOST:]file2` - Destination file
> 
> Local files should be specified using an absolute or relative path, while remote file names should include a user and host specification

- 当需要传输目录时option 加上`-r`

- 当两个远端机器之间传送时，是两个远端之间直接发送的，文件不会经过执行scp命令的机器， 如果需要拷贝的文件经过执行scp命令的机器，加上`-3` option。

- 如果ssh端口不是22,使用`-P` option 执行ssh监听的端口。

Ref:
[How to Use SCP Command to Securely Transfer Files.](https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/)
