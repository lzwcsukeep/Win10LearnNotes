### 本文涉及信号在操作系统进程间的使用

### 信号的发送，接收，阻塞，自定义信号处理函数(handler)

#### 信号的发送

1. `/bin/kill` 程序可以发送任意信号给另一个进程

2. 从键盘发送信号

3. 用`kill` 函数发送信号

4. 用`alarm` 函数给自己发送信号

##### /bin/kill 发送任意信号给另一个进程

如：

```c
linux> /bin/kill -9 15312
```

发送信号9(SIGKILL)给进程15213，一个负的PID会使信号发送给进程组ID等于PID的进程组内每一个进程。

如：

```c
linux> /bin/kill -9 -15213
```

会发送信号9给进程组ID为15213的进程组内的每一个进程。

用绝对路径`/bin/kill` 是因为一些shell 会内置`kill` 命令，避免混淆。

#### 从键盘发送信号

unix shell 用job 这个概念来抽象一条命令行的执行，即一条命令行的评估执行可以用一个job表示，shell 为每个job 创建一个独立的进程组。通常该进程组ID 取自进程组内某个父进程的PID。

job 可以分为前台和后台运行。任何时刻最多只有一个前台job，0个或多个后台job.

一条 `command line` --->  一个job ---> 一个独立进程组

故上述一个job 可能会包含多个进程，如`ls -l | wc -l` 会产生`ls` 和 `wc` 两个进程。但它们属于同一个job 。`ls` 和 `wc` 属于一个进程组。

`CTRL + C` 会发送SIGINT 信号给<mark>前台job</mark> 内的每个进程，即整个前台job为整体，内部每个进程都会收到SIGINT

同理 

`CTRL + Z` 会发送SIGTSTP给<mark>前台job</mark> 内的每个进程，前台job 内每个进程都会收到SIGTSTP信号。

**进程组**

若干相关进程的集合形成一个进程组，每个进程都属于且仅仅属于一个进程组，该进程组由进程组ID标识。

```c
pid_t getpgrp(void)  // 返回调用进程的进程组id
```

默认情况下子进程属于父进程所在的进程组。

```c
int setpgid(pid_t pid,pid_t pgid); //设置进程pid的进程组id为pgid
```

把进程pid 的进程组设置为pgid，如果pid = 0 ,则使用当前进程的PID，如果pgid = 0 ，则使用pid指定的进程的PID 为进程组ID 。

如：

`setpgid(0,0)` ，调用进程pid=15312,那么该语句会创建一个新的进程组，其进程组ID为15213，并且将进程15213加入该进程组。

#### 用kill函数发送信号

`int kill(pid_t pid, int sig);`  //return 0 if OK ，-1 on error

如果pid>0 ,那么kill 函数会发送信号sig 给进程pid 。

如果pid = 0  ，那么kill 函数会发送信号sig 给调用进程所在进程组内的每个进程。

如果pid < 0 , 那么pid 指定的就是进程组|pid|(pid的绝对值)，即kill 函数会发送信号sig 给进程组ID 为|pid| 的进程组内的每个进程。

#### 用alarm函数发送信号

一个进程可以用`alarm` 函数给自己发送SIGALRM信号。

### 接收信号

当内核将进程p从内核态切换到用户态(如从系统调用返回或者完成上下文切换)时，它就会为进程p检查未被处理且未被屏蔽的信号的集合。

如果集合为空，内核就将控制权传递给进程p 的控制流的下一条指令($I_{next}$).

如果集合不空，内核就会选择某个信号k(通常最小的k)并强制p接受信号k，对信号的接收会触发一些action.一旦该进程完成该action ，控制权传给$I_{next}$ .

每个信号都有一些预定义的默认action，为下列之一：

- 终止进程

- 终止进程并dumps core

- 暂停进程并且等待信号SIGCONT来重启进程

- 忽视该信号

一个进程可以使用**signal**函数修改一个信号的默认action,唯二的例外是**SIGSTOP**和**SIGKILL**，这两者的默认action 不能修改。

### 屏蔽与解屏蔽信号

使用`sigprocmask`函数

内核中用两个位向量来表示待处理的信号和被屏蔽的信号

- 所有待处理的信号放在pending bit vector

- 所有被屏蔽的信号放在blocked bit vector

即bit k 代表的signal k的状态。

sigprocmask函数常常配合以下函数一起使用

#include <signal.h>

int sigemptyset(sigset_t *set)；

int sigfillset(sigset_t *set)；

int sigaddset(sigset_t *set, int signum)

int sigdelset(sigset_t *set, int signum)；

int sigismember(const sigset_t *set, int signum)；

其中

sigemptyset(sigset_t *set)初始化由set指定的信号集，信号集里面的所有信号被清空，相当于64为置0；

sigfillset(sigset_t *set)调用该函数后，set指向的信号集中将包含linux支持的64种信号，相当于64为都置1；

sigaddset(sigset_t *set, int signum)在set指向的信号集中加入signum信号，相当于将给定信号所对应的位置1；

sigdelset(sigset_t *set, int signum)在set指向的信号集中删除signum信号，相当于将给定信号所对应的位置0；

sigismember(const sigset_t *set, int signum)判定信号signum是否在set指向的信号集中，相当于检查给定信号所对应的位是0还是1。

阻塞信号的相关API详细用法看[linux系统编程之信号（五）：信号集操作函数，信号阻塞与未决 - mickole - 博客园](https://www.cnblogs.com/mickole/p/3191281.html)

#### 信号集速查表

<img title="" src="file:///E:/Files/LearnNotes/img_src/linux_signal_set.png" alt="" data-align="inline">
