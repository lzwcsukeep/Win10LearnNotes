原因是使用有限位数的二进制比特串连 0.1 这样简单的浮点数都不能精确表示。

## Comparing for equality

Floating point math is not exact. Simple values like 0.1 cannot be precisely represented using binary floating point numbers, and the limited precision of floating point numbers means that slight changes in the order of operations or the precision of intermediates can change the result. That means that comparing two floats to see if they are equal is usually not what you want. GCC even has a (well intentioned but misguided) warning for this: “warning: comparing floating point with == or != is unsafe”.

Here’s one example of the inexactness that can creep in:

```cpp
float f = 0.1f;
float sum;
sum = 0;
// 三种不同的计算1的方法得出的结果不一样
for (int i = 0; i < 10; ++i)
    sum += f;
float product = f * 10;
printf("sum = %1.15f, mul = %1.15f, mul2 = %1.15f\n",
        sum, product, f * 10);
```

This code tries to calculate ‘one’ in three different ways: repeated adding, and two slight variants of multiplication. Naturally we get three different results, and only one of them is 1.0:

> sum=1.000000119209290, mul=1.000000000000000, mul2=1.000000014901161

Disclaimer: the results you get will depend on your compiler, your CPU, and your compiler settings, which actually helps make the point.

因此判断浮点数使用 == 是不可靠的，应该使用 fabs(a-b) < epsinon 的形式。epsinon是一个比较小的值，当两个浮点数的差小于这个值时认为相等。
