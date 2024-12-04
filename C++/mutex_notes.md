## difference between lock_guard and unique_lock

lock_guard 只会在构造的时候上锁一次，在析构的时候解锁一次。
但是unique_lock可以上锁和解锁多次,因此lock_guard不能配合条件变量使用。

> `lock_guard` can be constructed only once and destructed only once.
> Hence lock_guard cannot be used in combination with a condition variable,
> but a unique_lock can be (because unique_lock can be locked and unlocked several times).

- One missing difference is: std::unique_lock can be moved but std::lock_guard can't be moved.

---

lock_guard and unique_lock are pretty much the same thing; lock_guard is a restricted version with a limited interface.

A `lock_guard` always holds a lock from its construction to its destruction. A `unique_lock` can be created without immediately locking, 
can unlock at any point in its existence, and can transfer ownership of the lock from one instance to another.

So you always use `lock_guard`, unless you need the capabilities of `unique_lock`. A condition_variable needs a `unique_lock`.

---

Use lock_guard unless you need to be able to manually unlock the mutex in between without destroying the lock.

In particular, condition_variable unlocks its mutex when going to sleep upon calls to wait. That is why a lock_guard is not sufficient here.
使用lock_guard除非你想在中间手动的unlock互斥锁.

---

所以结论就是lock_guard 功能比 unique_lock少，但是开销也小。lock_guard 只会在创建的时候加一次锁，在析构的时候解锁。
但是unique_lock可以做到lock_guard上述功能之外还可以在中途手动unlock(),还可以move转移锁的所有权。

## 条件变量

A condition variable is an object able to block the calling thread until notified to resume.
一个条件变量是一个对象，它可以阻塞调用进程直到进程被notified唤醒。

wait 函数就是用来阻塞进程的。使用的是一个unique_lock.被阻塞的进程(主调进程)会一直保持阻塞状态，直到被其他进程
调用notify()函数唤醒（在同一个条件变量下）。

调用wait()的进程被阻塞后会释放锁，被其他进程用notify()唤醒会重新获得锁.

wait()函数条件不满足，会阻塞，条件满足会继续执行。继续执行的情况下，不会释放锁。

## mutex 类提供的函数

lock() :             Lock mutex

unlock() ：     Unlock mutex

try_lock() :      Attempts to lock the mutex, without blocking

native_handle() :  Get native handle

---

try_lock()函数的含义：尝试获取锁，获取到就会执行lock()函数，没获取到也不会阻塞当前线程而是执行其他任务。判断有没有获取到就根据函数返回值来判断。当获取到锁时，从该时刻点到执行unlock()函数之间，线程持有该互斥锁。

函数完成描述：来自cplusplus

`bool try_lock();`

**Lock mutex if not locked**

Attempts to lock the [mutex](https://cplusplus.com/mutex), without blocking:  

- If the [mutex](https://cplusplus.com/mutex) isn't currently *locked* by any thread, the calling thread *locks* it (from this point, and until its member [unlock](https://cplusplus.com/mutex::unlock) is called, the thread *owns* the [mutex](https://cplusplus.com/mutex)).
- If the [mutex](https://cplusplus.com/mutex) is currently locked by another thread, the function fails and returns `false`, without blocking (the calling thread continues its execution).
- If the [mutex](https://cplusplus.com/mutex) is currently locked by the same thread calling this function, it produces a *deadlock* (with *undefined behavior*). See [recursive_mutex](https://cplusplus.com/recursive_mutex) for a *mutex type* that allows multiple locks from the same thread.

This function may fail spuriously when no other thread has a *lock* on the [mutex](https://cplusplus.com/mutex), but repeated calls in these circumstances shall succeed at some point.

---

code helps you understand: from cppreference

```c
#include <chrono>
#include <iostream> // std::cout
#include <mutex>
#include <thread>
 
std::chrono::milliseconds interval(100);
 
std::mutex mutex;
int job_shared = 0; // both threads can modify 'job_shared',
                    // mutex will protect this variable
 
int job_exclusive = 0; // only one thread can modify 'job_exclusive'
                       // no protection needed
 
// this thread can modify both 'job_shared' and 'job_exclusive'
void job_1() 
{
    std::this_thread::sleep_for(interval); // let 'job_2' take a lock
 
    while (true)
    {
        // try to lock mutex to modify 'job_shared'
        if (mutex.try_lock())
        {
            std::cout << "job shared (" << job_shared << ")\n";
            mutex.unlock();
            return;
        }
        else
        {
            // can't get lock to modify 'job_shared'
            // but there is some other work to do
            ++job_exclusive;
            std::cout << "job exclusive (" << job_exclusive << ")\n";
            std::this_thread::sleep_for(interval);
        }
    }
}
 
// this thread can modify only 'job_shared'
void job_2() 
{
    mutex.lock();
    std::this_thread::sleep_for(5 * interval);
    ++job_shared;
    mutex.unlock();
}
 
int main() 
{
    std::thread thread_1(job_1);
    std::thread thread_2(job_2);
 
    thread_1.join();
    thread_2.join();
}
```

possible output:

```
job exclusive (1)
job exclusive (2)
job exclusive (3)
job exclusive (4)
job shared (1)
```