# lock_guard和unique_lock区别

lock_guard 只是利用RAII最简单的实现，仅仅构造函数执行mutex的lock()，析构函数执行mutex的unlock()。除此之外没有其他封装和定义。

但是unique_lock就不同，定义了自己的lock()和unlock()函数，这两个函数内部分别调用mutex的lock()和unlock()。然后再在自己的构造和析构中调用自己的成员函数lock()和unlock()实现RAII。除此之外还实现了其他的。

# 相同点

lock_guard 和 unique_lock 都是模板类，模板参数都接收一个Mutex类型(互斥锁类型)

> （模板类也是类，也可以创建对象。只是需要加一个类参数。类参数确定之后，整个类型就确定了。）

并且lock_guard 和 unique_lock都会在构造对象时加锁，在析构对象时解锁。构造lock_guard和unique_lock对象时各自的构造函数都需要传入一个已经存在的Mutex 对象。

### lock_guard 和unique_lock 是如何在构造时加锁，析构时解锁的？

上面的模板参数不是传入了一个Mutex类型的类参数吗，并且构造时不是传入了Mutex对象吗，上述的加锁即调用已经存在的Mutex对象的lock()方法，而解锁则是调用Mutex对象的unlock()方法。

上述这种实现(lock_guard以及unique_lock都运用了)即是RAII术语的运用。可以保证在异常的情况下也让锁释放，从而避免死锁。

# 不同点

lock_guard 仅仅就只是在构造函数时调用了传进来的Mutex对象的lock()方法和在析构函数中调用了Mutex对象的unlock()方法。除此之外lock_guard没有其他成员函数实现。

但是unique_lock实现的更多，首先unique_lock封装了Mutex对象的lock()方法以及unlock()方法成为自己类的成员方法lock()和unlock()。然后再构造函数中调用自己的lock(),析构中调用自己的unlock()。也就是间接的调用Mutex的lock()和unlock()。

由于封装了自己的unlock()和lock()函数，那么unique_lock 对象就可以再其生命周期之间手动的执行unlock和lock来加锁和解锁。相对的lock_guard 就只在构造时加锁，析构时解锁，中间不能手动加解锁。

看unique_lock源码实现

```cpp
// unique_lock 构造
      explicit unique_lock(mutex_type& __m)
      : _M_device(std::__addressof(__m)), _M_owns(false)
      {
    lock();  // 自己的成员函数lock()
    _M_owns = true;
      }
//unique_lock的 析构
      ~unique_lock()
      {
    if (_M_owns)
      unlock(); // 自己的成员函数unlock()
      }

// unique_lock的成员函数lock,其调用了mutex的lock()
      void
      lock()
      {
    if (!_M_device)
      __throw_system_error(int(errc::operation_not_permitted));
    else if (_M_owns)
      __throw_system_error(int(errc::resource_deadlock_would_occur));
    else
      {
        _M_device->lock();  // <----
        _M_owns = true;
      }
      }
// unique_lock的成员函数unlock,其调用了mutex的unlock()
      void
      unlock()
      {
    if (!_M_owns)
      __throw_system_error(int(errc::operation_not_permitted));
    else if (_M_device)
      {
        _M_device->unlock();
        _M_owns = false;
      }
      }
```

从vscode 中截图出来的unique_lock模板类中含有的成员列表
![](..\img_src\unique_lock类符号表.png)

同样的看lock_guard模板类中含有的成员列表就很少。成员函数只有构造和析构函数。

![](..\img_src\lock_guard类成员列表.png)

lock_guard模板类源码

```cpp
template<typename _Mutex>
    class lock_guard
    {
    public:
      typedef _Mutex mutex_type;
// 构造函数
      explicit lock_guard(mutex_type& __m) : _M_device(__m)
      { _M_device.lock(); } 

      lock_guard(mutex_type& __m, adopt_lock_t) noexcept : _M_device(__m)
      { } // calling thread owns mutex
// 析构函数
      ~lock_guard()
      { _M_device.unlock(); }

      lock_guard(const lock_guard&) = delete;
      lock_guard& operator=(const lock_guard&) = delete;

    private:
      mutex_type&  _M_device;
    };
```

从以上可知，lock_guard 只是利用RAII最简单的实现，仅仅构造函数执行mutex的lock()，析构函数执行mutex的unlock()。除此之外没有其他封装和定义。

但是unique_lock就不同，定义了自己的lock()和unlock()函数，这两个函数内部分别调用mutex的lock()和unlock()。然后再在自己的构造和析构中调用自己的成员函数lock()和unlock()实现RAII。除此之外还实现了其他的。
