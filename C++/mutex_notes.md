## difference between lock_guard and unique_lock

lock_guard 只会在构造的时候上锁一次，在析构的时候解锁一次。
但是unique_lock可以上锁和解锁多次,因此lock_guard不能配合条件变量使用。

>`lock_guard` can be constructed only once and destructed only once.
Hence lock_guard cannot be used in combination with a condition variable,
but a unique_lock can be (because unique_lock can be locked and unlocked several times).

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