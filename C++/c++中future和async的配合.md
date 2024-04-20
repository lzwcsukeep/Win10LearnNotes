future 可以用来保存另一个线程执行的结果。

配合async函数使用,async函数的返回值是一个future。

async 函数异步执行一个指定函数，然后将该函数的返回值保存进future中。

future是一个模板类，模板参数类型表示保存的数据类型，即使用get()返回的结果的数据类型。

下面是代码示例：

```cpp
// future example
#include <iostream>       // std::cout
#include <future>         // std::async, std::future
#include <chrono>         // std::chrono::milliseconds

// a non-optimized way of checking for prime numbers:
bool is_prime (int x) {
  for (int i=2; i<x; ++i) if (x%i==0) return false;
  return true;
}

int main ()
{
  // call function asynchronously:
  std::future<bool> fut = std::async (is_prime,444444443); 

  // do something while waiting for function to set future:
  std::cout << "checking, please wait";
  std::chrono::milliseconds span (100);
  while (fut.wait_for(span)==std::future_status::timeout)
    std::cout << '.' << std::flush;

  bool x = fut.get();     // retrieve return value

  std::cout << "\n444444443 " << (x?"is":"is not") << " prime.\n";

  return 0;
}
```
