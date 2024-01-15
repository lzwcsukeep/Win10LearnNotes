## Eigen库的使用

### 什么是eigen库？

官方一句话描述：Eigen - A C++ template library for linear algebra

即为线性代数提供的C++ 模板库。

> Eigen is a C++ template library for linear algebra operations. It provides a set of efficient and flexible classes and functions for performing common linear algebra operations, such as matrix and vector operations, decompositions, and solving linear systems.

提供了线性代数相关的操作。包括矩阵和向量操作，矩阵的分解和求解线性问题。

下面给出一些十分简单的代码示例，可以看到该库提供了如何定义矩阵和向量，然后支持矩阵和向量的加法乘法，线性运算等。

### 如何编译？

只需要找到头文件就行。使用 `-I` 参数指定 eigen 头文件所在目录。

There is no library to link to. The only thing that you need to keep in mind when compiling the above program is that the compiler must be able to find the [Eigen](https://eigen.tuxfamily.org/dox/namespaceEigen.html "Namespace containing all symbols from the Eigen library.") header files. The directory in which you placed [Eigen](https://eigen.tuxfamily.org/dox/namespaceEigen.html "Namespace containing all symbols from the Eigen library.")'s source code must be in the include path. With GCC you use the `-I` option to achieve this, so you can compile the program with a command like this:

```shell
g++ -I /path/to/eigen/ my_program.cpp -o my_program
```

示例1：

```cpp
// 示例1
#include <iostream>
#include <Eigen/Dense>

using Eigen::MatrixXd;

int main()
{
  MatrixXd m(2,2);
  m(0,0) = 3;
  m(1,0) = 2.5;
  m(0,1) = -1;
  m(1,1) = m(1,0) + m(0,1);
  std::cout << m << std::endl;
}
```

示例2 ：

```cpp
// 示例2
#include <iostream>
#include <Eigen/Dense>

using Eigen::MatrixXd;
using Eigen::VectorXd;

int main()
{
  MatrixXd m = MatrixXd::Random(3,3);
  m = (m + MatrixXd::Constant(3,3,1.2)) * 50;
  std::cout << "m =" << std::endl << m << std::endl;
  VectorXd v(3);
  v << 1, 2, 3;
  std::cout << "m * v =" << std::endl << m * v << std::endl;
}
```

示例1：输出

```cpp
3 -1

2.5 1.5
```

示例2：输出

```cpp
m =
94.0188  89.844 43.5223
49.4383 101.165  86.823
88.3099 29.7551 37.7775
m * v =
404.274
512.237
261.153
```

官方的开始文档:[Eigen: Getting started](https://eigen.tuxfamily.org/dox/GettingStarted.html)

官方主页： https://eigen.tuxfamily.org/index.php?title=Main_Page
