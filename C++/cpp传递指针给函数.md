### C++ 传递指针给函数

C++ 允许传递指针给函数，只需要简单的声明函数参数为指针类型即可。

下面的实例中，我们传递一个无符号的 long 型指针给函数，并在函数内改变这个值：

```cpp
#include <iostream>
#include <ctime>

using namespace std;

// 在写函数时应习惯性的先声明函数，然后在定义函数
void getSeconds(unsigned long *par);

int main ()
{
   unsigned long sec;


   getSeconds( &sec );

   // 输出实际值
   cout << "Number of seconds :" << sec << endl;

   return 0;
}

void getSeconds(unsigned long *par)
{
   // 获取当前的秒数
   *par = time( NULL );
   return;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```context
Number of seconds :1294450468
```

能接受指针作为参数的函数，也能接受数组作为参数，如下所示：

```cpp
#include <iostream>
using namespace std;

// 函数声明
double getAverage(int *arr, int size);

int main ()
{
   // 带有 5 个元素的整型数组
   int balance[5] = {1000, 2, 3, 17, 50};
   double avg;

   // 传递一个指向数组的指针作为参数
   avg = getAverage( balance, 5 ) ;

   // 输出返回值
   cout << "Average value is: " << avg << endl; 

   return 0;
}

double getAverage(int *arr, int size)
{
  int    i, sum = 0;       
  double avg;          

  for (i = 0; i < size; ++i)
  {
    sum += arr[i];
   }

  avg = double(sum) / size;

  return avg;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```cpp
Average value is: 214.4
```

参考链接：[C++ 传递指针给函数 | 菜鸟教程](https://www.runoob.com/cplusplus/cpp-passing-pointers-to-functions.html)
