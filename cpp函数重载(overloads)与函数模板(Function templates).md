### C++ 函数重载(overloads)

**函数重载（Overloaded functions）：** 两个函数名字相同但是**参数的个数不同**或者**参数表中某个参数的类型不同。**

```cpp
// overloading functions
#include <iostream>
using namespace std;

int operate (int a, int b)
{
  return (a*b);
}

double operate (double a, double b)
{
  return (a/b);
}

int main ()
{
  int x=5,y=2;
  double n=5.0,m=2.0;
  cout << operate (x,y) << '\n';
  cout << operate (n,m) << '\n';
  return 0;
}
```

output

```context
10
2.5
```

注：仅仅只有返回值不同不能构成函数重载,至少得参数列表有不同。

---

### 函数模板（Function templates）

多个重载的函数可能会有相同的定义。比如：

```cpp
// overloaded functions
#include <iostream>
using namespace std;

int sum (int a, int b)
{
  return a+b;
}

double sum (double a, double b)
{
  return a+b;
}

int main ()
{
  cout << sum (10,20) << '\n';
  cout << sum (1.0,1.5) << '\n';
  return 0;
}
```

output

```context
30
2.5
```

这里的`sum` 函数被不同的参数类型重载，但是有同样的函数体。

并且 `sum` 还可以被更多的不同的参数类型重载，且保持相同的函数体。对于这种情况C++有能力为通用类型定义函数，这就是函数模板( *function templates*) 。

函数模板需要用到关键字`template` ，

语法为 ：template <template-parameters> function-declaration

> Defining a function template follows the same syntax as a regular function, except that it is preceded by the `template` keyword and a series of template parameters enclosed in angle-brackets <>:

`sum` 可以定义为：

```cpp
template <class SomeType>
SomeType sum (SomeType a, SomeType b)
{
  return a+b;
}
```

使用时指定一下类型参数：

```cpp
x = sum<int>(10,20);
```

实际例子：

```cpp
// function template
#include <iostream>
using namespace std;

template <class T>
T sum (T a, T b)
{
  T result;
  result = a + b;
  return result;
}

int main () {
  int i=5, j=6, k;
  double f=2.0, g=0.5, h;
  k=sum<int>(i,j);
  h=sum<double>(f,g);
  cout << k << '\n';
  cout << h << '\n';
  return 0;
}
```

output

```context
11
2.5
```

由于编译器可以做类型推断，因此在使用函数模板时，甚至可以省略模板参数。

```cpp
//未省略模板参数
k = sum<int> (i,j);
h = sum<double> (f,g);
//和上面同样效果，省略模板参数
k = sum (i,j);
h = sum (f,g);
```

多个模板参数：

```cpp
// function templates
#include <iostream>
using namespace std;

template <class T, class U>
bool are_equal (T a, U b)
{
  return (a==b);
}

int main ()
{
  if (are_equal(10,10.0))
    cout << "x and y are equal\n";
  else
    cout << "x and y are not equal\n";
  return 0;
}
```

output

```context
x and y are equal
```

上述使用了类型推断，省略了模板参数

**确定类型的模板参数**

```cpp
// template arguments
#include <iostream>
using namespace std;

template <class T, int N>
T fixed_multiply (T val)
{
  return val * N;
}

int main() {
  std::cout << fixed_multiply<int,2>(10) << '\n';
  std::cout << fixed_multiply<int,3>(10) << '\n';
}
```

output

```context
20
30
```

注：第二个模板参数在编译期确定且只能给常量，不能给变量。

参考链接：[Overloads and templates - C++ Tutorials](https://www.cplusplus.com/doc/tutorial/functions2/)
