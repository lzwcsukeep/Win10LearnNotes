# The usage of async function in C++

> async 函数是一个函数模板，你可以用来异步地创建和执行一个任务，并且可以在调用async的线程中使用future对象，获得异步执行的任务结果。future对象调用get()会阻塞当前线程直到异步任务结束并返回结果。因此async()函数配合future可以实现多个线程之间的异步配合。async()参数可以传入一个函数名表示异步任务执行的内容。具体使用往下看。

C++ `std::async` is a function template that allows you to create and launch asynchronous tasks. It is part of the `<future>` header in the C++ Standard Library. Here's how you can use `std::async`:

1. Include the necessary headers:

```cpp
#include <future>
#include <iostream>
```

2. Define a function or lambda that represents the task you want to run asynchronously. For example:

```cpp
int addNumbers(int a, int b) {
    return a + b;
}
```

3. Create a `std::future` object that will hold the result of the asynchronous task:

```cpp
std::future<int> futureResult;
```

4. Use `std::async` to launch the task asynchronously and assign the returned `std::future` to the previously created object:

```cpp
futureResult = std::async(addNumbers, 5, 10);
```

Here, `addNumbers` is the function or lambda you want to run asynchronously, and `5` and `10` are the arguments to be passed to that function.

5. You can now perform other operations while the task is running. When you need the result of the task, you can use the `std::future` object to retrieve it. For example:

```cpp
// Do some other work

// Wait for the task to complete and get the result
int result = futureResult.get();

std::cout << "Result: " << result << std::endl;
```

The `get()` function blocks until the asynchronous task completes and returns the result.

Alternatively, you can use `std::future::wait()` to wait for the task to complete without retrieving the result immediately. This can be useful if you want to perform other operations while waiting for the task to finish.

```cpp
futureResult.wait();  // Wait for the task to complete

// Do some other work

// Get the result
int result = futureResult.get();
```

Note that `std::async` may choose different strategies to execute the task, depending on the system and available resources. It can execute the task asynchronously in a separate thread or defer the execution until you call `get()` or `wait()`.

Remember to handle any exceptions that may occur within the asynchronous task by using try-catch blocks or appropriate error handling mechanisms.

That's a basic example of how to use `std::async` in C++. You can explore additional features and options provided by `std::async`, such as specifying a launch policy or using `std::future` to check if the task is ready without blocking.

注意：

1. get()函数会阻塞当前进程，直到异步的任务完成并且返回结果。

2. async()异步执行的任务不一定是在另一个线程中执行，系统会根据当前情况选择另开一个线程执行或者直接在当前线程执行。

完整代码：

```cpp
#include <future>
#include <iostream>
#include <chrono>

using namespace std ;
// 定义一个函数，用于异步执行的任务
int addNumbers(int a, int b) {
    std::this_thread::sleep_for(std::chrono::seconds(3)) ;
    return a + b;
}

int main() {
    // 创建一个std::future对象，用于保存异步任务的结果
    std::future<int> futureResult;

    // 使用std::async异步地启动任务，并将返回的std::future赋值给之前创建的对象
    futureResult = std::async(addNumbers, 5, 10);

    // 执行其他操作
    for (int i = 0 ; i < 10; i++)
    {
        cout << "i = " << i << endl ;
    }
    // 等待任务完成并获取结果
    int result = futureResult.get();  // get()会阻塞当前线程，直到获得异步线程的结果。
    cout << "before get result" << endl ;    
    cout << "结果：" << result << std::endl;

    return 0;
}
```
