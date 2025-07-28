# C++ 模板的新用法

### Non-Type Template Parameter

Non-Type Template Parameter: 就是C++ 的模板不一定要传类型，还可以传非类型的参数。

直接上代码。

```cpp
// templatefunarg.cpp
#include <iostream>
#include <string>

class MyObject {
public:

    template <int size>
    void set() {
        int arr[size] ;
        printf("sizeof arr is %ld\n", sizeof(arr)/sizeof(int)) ;
    }
};



int main() {
    MyObject obj;
    obj.set<6>();  // 👈 传整数作为模板参数
    obj.set<5>();  // 👈 传整数作为模板参数
    obj.set<3>();  // 👈 传整数作为模板参数
    return 0;
}
```

输出结果：

```shell
wt400@ubuntu20:~/personal_improve$ g++ templatefunarg.cpp 
wt400@ubuntu20:~/personal_improve$ ./a.out 
sizeof arr is 6
sizeof arr is 5
sizeof arr is 3
```

以前模板的用法，大多像vector一样， 传递的是类型。比如vector<int> , vector<string>,

vector<vector<int>> ...， 但实际上模板还可以传递具体的值，比如上面的obj.set<5> ,obj.set<6>... 神奇的用法。

对于非类型的模板参数，Following are the arguments that are allowed:

- Constant Expressions
- Addresses of function or objects with external linkage
- Addresses of static class members.

### **神奇的是传递函数名，直接看代码：**

```cpp
#include <iostream>

class MyObject {
public:
    template <void (*Callback)()>
    void set() {
        std::cout << "Running callback..." << std::endl;
        Callback();
    }
};

void sayHello() {
    std::cout << "Hello!" << std::endl;
}

void sayGoodbye() {
    std::cout << "Goodbye!" << std::endl;
}

int main() {
    MyObject obj;

    obj.set<sayHello>();   // 实例化出一个 set<sayHello>()
    obj.set<sayGoodbye>(); // 实例化出另一个 set<sayGoodbye>()

    return 0;
}
```

输出：

```cpp
Running callback...
Hello!
Running callback...
Goodbye!
```

你会发现，主函数里面同样调用的是set() 函数，但是实际执行的函数非常不同。而实际执行的函数是通过模板参数传递过去的，如同第一个例子传递的整数 6,5 一样。

### 还可以传带参数的函数名过去

代码这么写：

```cpp
#include <iostream>
#include <string>

class MyObject {
public:
    // Callback 是一个带参数的函数指针
    template <void (*Callback)(int, const std::string&)>
    void set(int x, const std::string& msg) {
        Callback(x, msg);  // 传参调用
    }
};

void greet(int times, const std::string& name) {
    for (int i = 0; i < times; ++i) {
        std::cout << "Hello, " << name << "!" << std::endl;
    }
}

int main() {
    MyObject obj;
    obj.set<greet>(3, "Alice");  // 👈 传函数名作为模板参数，其他参数作为实参
    return 0;
}

```

## 总结

### 专业术语解释：

1. **非类型模板参数（Non-Type Template Parameter）**：
   
   - 模板参数不一定是 `typename` 或 `class`，也可以是具体的值，比如：
     
     - 整数：`template<int N>`
     
     - 函数指针：`template<void (*Callback)(int, const std::string&)>`
     
     - 对象指针、枚举值、引用等（C++11/C++17 后支持更多）

2. **函数指针作为模板参数**：
   
   - 在 `set<greet>()` 里，`greet` 是一个全局函数的名字，其本质是一个函数指针常量，类型为 `void (*)(int, const std::string&)`
   
   - 编译器会将 `greet` 的地址传入模板实例

3. **模板实例化（Template Instantiation）**：
   
   - 每个不同的 `Callback`（如 `greet`, `foo`, `bar`）都会实例化出不同的 `set<>()` 模板版本
   
   - 这是一种 **静态多态（static polymorphism）**

### 所以这种用法的本质是：

> **在编译期将函数指针作为模板参数传入，通过模板实例化“定制化”不同的行为函数。**

### 好处是什么？

📌 实际用途和优势：

- ✅ 静态绑定，无运行时开销

- ✅ 编译期类型安全

- ✅ 适合事件注册、策略模式、有限状态机等设计

- ❌ 不适用于 lambda（有捕获）、成员函数，除非做封装

也就是说，**每个函数是“固定绑定”的，模板参数一旦传进去就是硬编码的**。这就是为什么这种写法非常适合用于：

- **编译期注册系统**

- **事件处理器**

- **策略切换**

- **静态分发**（避免虚函数/函数指针的运行时开销)
