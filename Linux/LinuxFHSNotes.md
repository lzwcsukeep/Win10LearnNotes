**Linux 文件中的按两种属性大分类**

Linux中的文件可以用两种独立的属性来区分文件：**shared** vs. **unshared** and **variable** vs. **static**。通常来说在这两者中任一地方有区别的文件都应该保存到不同的目录。

- `shareable` 文件 是那些可以保存在一台机器上并被其他机器使用的文件。`unshareable`文件是那些不是`shareable`的文件。比如在用户家目录下的文件是`shareable`文件，但是设备锁文件不是。

- `static` 文件包括二进制文件，库文件，文档文件以及其他没有管理员干预不会改变的文件。`variable` 文件是不是`static`的文件.

> This standard assumes that the operating system underlying an FHS-compliant file system supports the same basic security features found in most UNIX filesystems.
> 
> It is possible to define two independent distinctions among files: shareable vs. unshareable and variable vs. static. In general, files that differ in either of these respects should be located in different directories. This makes it easy to store files with different usage characteristics on different filesystems.
> 
> "Shareable" files are those that can be stored on one host and used on others. "Unshareable" files are those that are not shareable. For example, the files in user home directories are shareable whereas device lock files are not.
> 
> "Static" files include binaries, libraries, documentation files and other files that do not change without system administrator intervention. "Variable" files are files that are not static.

----

理由：

Shareable files can be stored on one host and used on several others. Typically, however, not all files in the filesystem hierarchy are shareable and so each system has local storage containing at least its unshareable files. It is convenient if all the files a system requires that are stored on a foreign host can be made available by mounting one or a few directories from the foreign host.

Static and variable files should be segregated because static files, unlike variable files, can be stored on read-only media and do not need to be backed up on the same schedule as variable files.

Historical UNIX-like filesystem hierarchies contained both static and variable files under both `/usr` and `/etc`. In order to realize the advantages mentioned above, the `/var` hierarchy was created and all variable files were transferred from `/usr` to `/var`. Consequently `/usr` can now be mounted read-only (if it is a separate filesystem). Variable files have been transferred from `/etc` to `/var` over a longer period as technology has permitted.

Here is an example of a FHS-compliant system. (Other FHS-compliant layouts are possible.)

|          | shareable         | unshareable |
| -------- | ----------------- | ----------- |
| static   | `/usr`            | `/etc`      |
|          | `/opt`            | `/boot`     |
| variable | `/var/mail`       | `/var/run`  |
|          | `/var/spool/news` | `/var/lock` |

---

参考链接：[Chapter 2. The Filesystem](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/ch02.html)
