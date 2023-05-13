### C++ 笔记之

#### 函数实参的值传递(passed by value)和引用传递(passed by reference)

**passed by value**

在早期的函数里面，函数实参通常都是值传递的。这个意思是当调用某个函数时，传递给函数形参的是函数实参的复本，而不是函数实参本身。

比如

```cpp
int x=5, y=3, z;
z = addition(x,y) ;
```

上述代码中，函数 `addition` 接收的是`x,y` 的副本5和3。 5，3用来初始化函数`addition` 的形参，函数内部任何对这些变量的修改都不会影响函数外面`x`,`y` 的值。因为 `x, y` 本身并没有传递给函数，传递的是`x, y` 的副本。

**passed by reference**

在一些场景里，需要一些函数能够在其内部修改外部的变量，这个时候可以使用**引用传递** 

比如函数`duplicate` 内的代码是将其三个参数翻倍，导致函数调用时传递的三个实参变量真正被修改。

```cpp
// passing parameters by reference
#include <iostream>
using namespace std;

void duplicate (int& a, int& b, int& c)
{
  a*=2;
  b*=2;
  c*=2;
}

int main ()
{
  int x=1, y=3, z=7;
  duplicate (x, y, z);
  cout << "x=" << x << ", y=" << y << ", z=" << z;
  return 0;
}
```

output

```context
x=2, y=6, z=14
```

为了能够访问函数的实参，函数需要将自己的形参声明为引用(reference),方式是在参数的类型后面加 (&)的符号。正如上面`duplicate` 的形参。

当通过**引用传递**(passed by reference)变量时，传递的不再是副本，而是变量本身，即由函数参数标识的变量，以某种方式与传递给函数的参数相关联，**对函数中相应局部变量的任何修改都会反映在函数调用中作为参数传递的变量中。**

事实上，`a、b和c`成为传递给函数调用的参数（`x、y和z`）的别名，函数中`a`的任何更改实际上都是在函数外修改变量`x`。`b`上的任何更改都会修改`y`，`c`上的任何更改都会修改`z`。这就是为什么在本例中，当function `duplicate`修改变量`a、b和c`的值时，`x、y和z`的值会受到影响。

如果将函数 `duplicate` 定义由下面：

```cpp
void duplicate (int& a, int& b, int& c) 
```

改成

```cpp
void duplicate (int a, int b, int c)
```

那么变量就不是引用传递而是值传递。值传递情况下，程序的输出将是未修改的`x,y,z`的值，即1，3，和7。

#### 效率考量和const 引用(const references)

函数调用的值传递需要拷贝实参，这对基本类型，如`int`来说不是很大的开销,如果参数是大的复合类型，这就有可能造成一定的开销。

如

```cpp
string concatenate (string a, string b)
{
  return a+b;
}
```

如果传递给参数a,b是长字符串，则可能意味着为函数调用复制大量数据。

但是可以用引用传递避免这个问题：

```cpp
string concatenate (string& a, string& b)
{
  return a+b;
}
```

引用传递的参数不需要复制。函数直接对作为参数传递的字符串（别名）进行操作，最多可能意味着将某些指针传递给函数。在这方面，这个版本的`concatenate`会比值传递的版本效率更高。

**如果引用传递的函数只需要访问实参但是不需要修改实参的值，可以用`const` 来确保这一点。**

```cpp
string concatenate (const string& a, const string& b)
{
  return a+b;
}
```

通过将它们限定为`const`，函数禁止修改a和b的值，但实际上可以作为引用（参数的别名）访问它们的值，而不必实际制造这些实参的副本。

总结：

1. 值传递需要复制实参，传递副本给函数。函数内代码对参数的修改不会影响实参。

2. 引用传递，传递的是实参的地址。函数内代码对参数的修改会真的修改实参的值。函数声明的参数类型后跟着 `&` 符号。

3. 如果函数函数传递的是大的对象，值传递会有复制的开销，这时候用引用传递可以降低开销。

4. 如果引用传递只需要访问实参(读取),而不用修改实参，则可以用`const` 修饰参数，确保这一点。
