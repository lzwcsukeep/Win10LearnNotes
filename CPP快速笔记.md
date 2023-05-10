**CPP  静态成员(static member)**

同Java 一样 ，一个类的静态成员也是这个类的所有对象共享同一份

---

静态成员分静态变量和静态函数

C++ 中静态变量为了避免声明多次，**将类的静态变量的初始化放在了类外**，而不能在类里面。

C++ 中静态成员函数只能访问其他静态成员，不能访问非静态成员(变量和函数都不行，和java 一样)。静态成员函数内也不能使用`this` 关键字。

### Const member functions

当一个类对象被const 修饰，该对象只能调用类中被const 修饰的成员函数。

When an object of a class is qualified as a `const` object:  

|     | ```const MyClass myobject;``` |     |
| --- | ----------------------------- | --- |

**The access to its data members from outside the class is restricted to read-only**, as if all its data members were `const` for those accessing them from outside the class. Note though, that the constructor is still called and is allowed to initialize and modify these data members:

> The member functions of a `const` object can only be called if they are themselves specified as `const` members;

 To specify that a member is a `const` member, the `const` keyword shall follow the function prototype, after the closing parenthesis for its parameters:  

|     | ```int get() const {return x;}``` |
| --- | --------------------------------- |

> Member functions specified to be `const` cannot modify non-static data members nor call other non-`const` member functions. In essence, `const` members shall not modify the state of an object.  

**`const` objects are limited to access only member functions marked as `const`, but non-`const` objects are not restricted and thus can access both `const` and non-`const` member functions alike.**

> Member functions can be overloaded on their constness: i.e., a class may have two member functions with identical signatures except that one is `const` and the other is not: in this case, the `const` version is called only when the object is itself const, and the non-`const` version is called when the object is itself non-`const`.

```cpp
// overloading members on constness
#include <iostream>
using namespace std;

class MyClass {
    int x;
  public:
    MyClass(int val) : x(val) {}
    const int& get() const {return x;}
    int& get() {return x;}
};

int main() {
  MyClass foo (10);
  const MyClass bar (20);
  foo.get() = 15;         // ok: get() returns int&
// bar.get() = 25;        // not valid: get() returns const int&
  cout << foo.get() << '\n';
  cout << bar.get() << '\n';

  return 0;
}
```

### 运算符重载（overloading operators）

运算符重载后本质上相当于执行了一个相应的函数。

运算符重载的格式：

`type operator sign (parameters) { /*... body ...*/ }`

`operator` 是关键字，`sign` 是 运算符符号,如 `+` 号 ，`type`  是返回值类型。

如果是以类成员函数 的形式重载 ： 有一个默认参数是调用这个**运算符函数** 的对象本身。

如果是非成员函数的形式重载：则没有默认参数，运算符左右两边的实参，分别对应声明时的左参数和右参数。

**member operator overloads**

```cpp
// overloading operators example
#include <iostream>
using namespace std;

class CVector {
  public:
    int x,y;
    CVector () {};
    CVector (int a,int b) : x(a), y(b) {}
    CVector operator + (const CVector&);
};

CVector CVector::operator+ (const CVector& param) {
  CVector temp;
  temp.x = x + param.x;
  temp.y = y + param.y;
  return temp;
}

int main () {
  CVector foo (3,1);
  CVector bar (1,2);
  CVector result;
  result = foo + bar;
  cout << result.x << ',' << result.y << '\n';
  return 0;
}
```

The function `operator+` of class `CVector` overloads the addition operator (`+`) for that type. Once declared, this function can be called either implicitly using the operator, or explicitly using its functional name:

```cpp
c = a + b;
c = a.operator+ (b);
```

Both expressions are equivalent.

**non-member operator overloads**

```cpp
// non-member operator overloads
#include <iostream>
using namespace std;

class CVector {
  public:
    int x,y;
    CVector () {}
    CVector (int a, int b) : x(a), y(b) {}
};


CVector operator+ (const CVector& lhs, const CVector& rhs) {
  CVector temp;
  temp.x = lhs.x + rhs.x;
  temp.y = lhs.y + rhs.y;
  return temp;
}

int main () {
  CVector foo (3,1);
  CVector bar (1,2);
  CVector result;
  result = foo + bar;
  cout << result.x << ',' << result.y << '\n';
  return 0;
}
```

参考链接：[Classes (II) - C++ Tutorials](https://www.cplusplus.com/doc/tutorial/templates/)
