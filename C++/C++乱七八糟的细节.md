# C++ 成员方法参数列表后用const修饰但是返回non-const 引用无法通过编译的原因。

实际代码如下：

```cpp
#include <iostream>
struct myarray
{
    char& operator[](size_t offset) const{
        return buffer[offset] ;
    }  

private:
    char buffer[10] ;
};

int main()
{

    return 0 ;
}
```

报错：binding reference of type ‘char&’ to ‘const char’ discards qualifiers
 5 | return buffer[offset] ;

```cpp
lizhiwen@lizhiwen:~/workspace/learn/exam$ g++ t2.cpp 
t2.cpp: In member function ‘char& myarray::operator[](size_t) const’:
t2.cpp:5:29: error: binding reference of type ‘char&’ to ‘const char’ discards qualifiers
    5 |         return buffer[offset] ;
      |                ~~~~~~~~~~~~~^
```

也就是用一个nonconst 的引用绑定了一个 const 的对象。这种是未定义行为，因此通不过编译，那么为什么会这样呢？

我们知道一个成员方法的参数列表后面加const表示这个方法不会修改属性。

原因在于非静态成员方法内含了一个this指针，而参数列表后的const实际上是通过修饰this指针来达到无法修改成员变量的。即 `const *this` ;

将上述代码还原有`this`指针的代码：

```cpp
// 原代码
#include <iostream>
struct myarray
{
    char& operator[](size_t offset) const{
        return buffer[offset] ;
    }  

private:
    char buffer[10] ;
};

// 加上this 指针
char& operator[](const myarry* this,size_t offset){
    return this->buffer[offset] ;
}

// this->buffer[offset] 是 const char 修饰的，但是返回值是char&
// 即回到了将const的变量绑定non-const上，这就是报错的原因
```

Notes:

- 成员函数签名后的const 是修饰this指针的。 

- 对数组加const结果是每个数组成员都是const。

## 函数形参const 和 non-const也构成函数重载
