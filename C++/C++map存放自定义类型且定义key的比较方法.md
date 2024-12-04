## map可以存放自定义类型

比如下面类型是可以放在map中的

```cpp
struct MyType{
    int id ;
    string name ;
};
```

如果想按某个属性排序，或多个属性排序，要怎么定义map呢？

1. **使用比较函数对象**：
- 定义一个比较函数对象，并将其传递给`std::map`的模板参数。

- 比较函数对象需要实现 `operator()` 函数，用于比较两个键的大小关系。

```c
#include <iostream>
#include <map>

// 自定义类型
struct MyType {
    int id;
    std::string name;
};

// 自定义比较函数对象
struct MyTypeComparator {
    bool operator()(const MyType& a, const MyType& b) const {
        // 根据自定义的比较规则比较两个 MyType 对象
        return a.id < b.id;
    }
};

int main() {
    // 使用自定义比较函数对象作为 map 的比较函数
    std::map<MyType, int, MyTypeComparator> myMap;

    // 插入元素
    MyType key1{1, "John"};
    myMap[key1] = 100;

    return 0;
}
```

2. **重载 `<` 运算符**：
- 在自定义类型中重载 `<` 运算符，定义键的比较规则。

- 在`std::map`的模板参数中使用默认的`std::less`比较函数对象。

```c
#include <iostream>
#include <map>

// 自定义类型
struct MyType {
    int id;
    std::string name;

    // 重载 < 运算符，定义比较规则
    bool operator<(const MyType& other) const {
        return id < other.id;
    }
};

int main() {
    // 使用默认的 std::less 比较函数对象
    std::map<MyType, int> myMap;

    // 插入元素
    MyType key1{1, "John"};
    myMap[key1] = 100;

    return 0;
}
```

这两种方法都可以实现自定义类型的比较规则，并在`std::map`中使用自定义的键值比较函数。选择哪种方法取决于你的个人偏好和代码风格。
