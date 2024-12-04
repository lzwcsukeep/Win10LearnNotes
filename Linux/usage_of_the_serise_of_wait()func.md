**name:**

wait, waitpid, waitid - wait for process to change state

**#include <sys/wait.h>**

```c
   pid_t wait(int *_Nullable wstatus);
   pid_t waitpid(pid_t pid, int *_Nullable wstatus, int options);

   int waitid(idtype_t idtype, id_t id, siginfo_t *infop, int options);
                   /* This is the glibc and POSIX interface; see
                      NOTES for information on the raw system call. */
```

## DESCRIPTION

       All of these system calls are used to wait for **state changes in a
       child of the calling process**, and obtain information about the
       child whose state has changed.  A state change is considered to
       be: the child terminated; the child was stopped by a signal; or
       the child was resumed by a signal.  In the case of a terminated
       child, performing a wait allows the system to release the
       resources associated with the child; if a wait is not performed,
       then the terminated child remains in a "zombie" state (see NOTES
       below).

这些所有的系统调用都是用来等待主调进程的一个子进程的状态改变，并且可以获取状态改变的子进程的信息。

一个状态改变可以视为：

子进程中止；

子进程被一个信号暂定；

子进程被一个信号唤醒。

在上述子进程中止的情况，执行一个wait()可以让系统释放掉该子进程的资源。如果一个wait()没有执行，那么终止的子进程会保持一个"僵尸"状态(zombie state)。

重点：父进程(调用wait()函数的进程）用来等待某个子进程状态改变。

详见Refer below：

[wait(2) - Linux manual page](https://man7.org/linux/man-pages/man2/waitpid.2.html)
