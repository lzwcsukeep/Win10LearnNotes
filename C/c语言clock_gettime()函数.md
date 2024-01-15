`clock_gettime` is a function in the C programming language that is used to retrieve the current time with a high precision clock. 
It is often used for measuring elapsed time or for profiling code. This function is part of the POSIX standard and is available on Unix-like operating systems.

函数原型：

```cpp
int clock_gettime(clockid_t clk_id, struct timespec *tp);
```

样例代码：

```cpp
#include <stdio.h>
#include <time.h>

int main() {
    struct timespec start, end;

    // Get the current time
    clock_gettime(CLOCK_MONOTONIC, &start);

    // Your code or operation to be measured goes here

    // Get the time again
    clock_gettime(CLOCK_MONOTONIC, &end);

    // Calculate the elapsed time in seconds and nanoseconds
    long seconds = end.tv_sec - start.tv_sec;
    long nanoseconds = end.tv_nsec - start.tv_nsec;

    // If the nanoseconds are negative, adjust the seconds and nanoseconds
    if (nanoseconds < 0) {
        seconds--;
        nanoseconds += 1000000000;  // 1 second in nanoseconds
    }

    printf("Elapsed time: %ld seconds, %ld nanoseconds\n", seconds, nanoseconds);

    return 0;
}

```

其中`CLOCK_MONOTONIC` 是一个 POSIX 标准定义的时钟标识符，用于在计算机系统上提供单调递增的时钟。这个时钟是相对于系统的某个固定起点，不受系统时间的调整影响，因此被称为"单调时钟"。
这意味着即使系统时间被调整，`CLOCK_MONOTONIC` 也不会受到影响。代码中使用这个时钟来测量程序运行开始和结束的两个时间点。

表示时间的结构体 `struct timespec` 含有两个long 类型的成员，一个表示秒，一个表示纳秒。定义在头文件 time.h 中

```cpp
 struct timespec {
               time_t   tv_sec;        /* seconds */
               long     tv_nsec;       /* nanoseconds */
           };
```
