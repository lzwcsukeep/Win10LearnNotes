### file descriptor

A file descriptor is a reference to an open file description;

### file description

an entry in the system-wide table of open files.  The open file description records the file offset and the file status flags.

### file node

| file descriptor                                  | file description                                                                             | file node        |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------- | ---------------- |
| 文件描述符在进程内部，是进程级别的，每个进程都有自己的文件描述符表，每个文件描述符指向了文件描述 | 文件描述是系统级别的打开文件表里的项，记录着文件的当前偏移量和权限信息，也叫打开文件对象，文件句柄等名称，从内核开发者角度是struct file;保存有指向file node 的指针 | 含有文件保存位置的指针的数据结构 |

也就是说 引用关系为  file descriptor  -> file description -> file node

NOTES:

The  term  open file description is the one used by POSIX to refer to the entries in the system-wide table of open files.  In other contexts, this object is variously also called an "open file object", a "file handle", an "open file table entry", or—in kernel-developer parlance—a struct file.

When a file descriptor is duplicated (using dup(2) or similar), the duplicate refers to the same open file description as the  original  file  descriptor, and the two file descriptors consequently share the file offset and file status flags.  Such sharing can also occur between processes: a child process created via fork(2) inherits duplicates of its parent's file descriptors, and those duplicates refer to the same open file  descriptions.

Each open() of a file creates a new open file description; thus, there may be multiple open file descriptions corresponding to a file inode.

On Linux, one can use the kcmp(2) KCMP_FILE operation to test whether two file descriptors (in the same process or in two different processes) refer to the same open file description.



询问chatGPT 关于 上三者的区别。在最后

[ChatGPT](https://chat.openai.com/share/6b548068-6635-4341-9773-a782507bb4c1)
