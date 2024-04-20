promise 和 future 配合来实现线程间的异步交流。

**Promise**: A `std::promise` object is used to store a value or an exception that will be provided asynchronously at some later time. It allows one thread (the "producer") to store a value that another thread (the "consumer") can retrieve asynchronously. The producer sets the value using `std::promise::set_value()` or an exception using `std::promise::set_exception()`.

```cpp
std::promise<int> myPromise;
// Set the value
myPromise.set_value(42);
```

**Future**: A `std::future` object is used to retrieve the value or exception stored in a corresponding `std::promise`. The future can be queried to check if the value is available, and if not, it can block until the value is available.

```cpp
// Get the future associated with the promise
std::future<int> myFuture = myPromise.get_future();
// Retrieve the value
int result = myFuture.get();
```

小总结：promise 使用set_value 来保存一个值，然后promise 会关联一个future对象，这个future对象假设为fu可以使用promise 的成员函数 get_future()获得。然后就可以通过这个future对象fu调用get() 方法获得上面promise 成员函数set_value()设置的值。

promise 对象会关联一个future 对象，这两者互相配合来实现异步交流。promise 是提供者，利用 future 可以获得promise 提供的数据。

示例代码：

```cpp
// promise example
#include <iostream>       // std::cout
#include <functional>     // std::ref
#include <thread>         // std::thread
#include <future>         // std::promise, std::future

void print_int (std::future<int>& fut) {
  int x = fut.get();
  std::cout << "value: " << x << '\n';
}

int main ()
{
  std::promise<int> prom;                      // create promise

  std::future<int> fut = prom.get_future();    // engagement with future

  std::thread th1 (print_int, std::ref(fut));  // send future to new thread

  prom.set_value (10);                         // fulfill promise
                                               // (synchronizes with getting the future)
  th1.join();
  return 0;
}
```

运行结果：

```
value: 10
```

这里的输出10 就是在主线程 promise 设置的，然后在另一个线程中使用future取得的。
