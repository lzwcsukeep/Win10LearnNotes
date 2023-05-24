- **linux 里面一切皆文件**

- **linux文件系统里面的一切事物都有一个唯一的inode来管理它的存储和属性**
  
  - 每一个文件，目录，特殊文件等等
  
  - 文件和目录都由inode来管理（目录也看成是一个文件）

- **目录将名字映射到inode**
  
  - 也就是说保存有若干文件的目录，目录本身只保存(文件名,inode)的二元组。多个文件的二元组以列表形式保存。

- **一个inode ，多个名字**
  
  - 一个文件由一个唯一的inode标识，inode是唯一的。但是在不同目录下可以有多个文件名映射到同一个inode。

- **inode 保存了什么？**
  
  - inode 包含指向磁盘块的指针
  
  - inode 包含文件的属性(所有者，权限，修改时间等等)
  
  - 注意：inode里面没有保存文件名，文件名是方便用户识别文件的，机器使用inode 识别，inode 里面不需要保存文件名。

- **inode 的特点**
  
  - inode在一个文件系统里是唯一的
  
  - inode 是一个固定容量的资源,随着文件数增多越用越少，inode不够了就无法创建文件
    
    - > every Linux file system is created new with a large set of available inodes. You can list the free inodes using `df -i`. Some types of Unix file systems can never make more inodes, even if there is lots of disk space available; when all the inodes are used up, the file system can create no more files until some files are deleted to free some inodes.

- **目录仅仅保存文件和inode数字**
  
  - > To make a hierarchical file system, file system names are stored in directories.
    > 
    > Each Unix directory is itself an inode. Like all inodes, directory inodes contain pointers to disk blocks and attribute information about the inode (permissions, owner, etc.), but what is stored in the disk blocks of a directory inode is not file data but directory data. That directly data is simply a list of names and inode numbers.
    > 
    > The directory inode contains attribute information about the *directory*, itself, not about the things named *in* the directory. (Use `ls -ld` to see the attributes of the directory inode itself.)
    > 
    > On Unix, for each thing in the file system, only the name and the inode number of the thing is kept in the directory. No data or attributes about the thing are kept in the directory, only the name of the thing and its inode number.
    > 
    > The *name* of a file is kept in a directory, paired with its inode number. The file’s actual attributes and pointers to disk blocks are kept elsewhere, in the inode for the file.
    > 
    > This means that names are not kept in the same inodes with the things that they name.
    > 
    > Directories are what give names to inodes on Unix. Directories can be thought of as “files containing lists of names and inode numbers”. Files have disk blocks containing file data; directories also have disk blocks; but, the blocks contain lists of names and inode numbers.
    > 
    > If a directory is damaged in Unix, only the names are lost, not any of the file data blocks or the file attributes.

由以上知识基本可以回答**liunx文件系统是如何根据文件名找到一个文件的?** 这个问题

- 某个文件一定是保存在某个目录下的。

- 那么在这个目录根据名字找到inode,如果下一级还是目录，就在根据名字找到inode

- 直到这个inode是文件而不是目录，那么inode里面就保存了该文件在磁盘上的地址，根据这个地址就读取文件。

注：关键点在于inode里面保存的东西，以及inode是一个文件的唯一标识，目录保存文件名到inode的映射二元组的列表。

参考链接：

https://teaching.idallen.com/cst8207/13w/notes/450_file_system.html
