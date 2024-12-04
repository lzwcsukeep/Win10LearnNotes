注：学C++模板先忘记Java里面的泛型。

C++ 里面有两种模板：

1. 函数模板

2. 类模板

`函数模板`是同一种操作(动作，执行流)可以适应多种类型，那么避免代码重复，我们就可以把类型参数化，定义一种通用的函数。使用的时候再指定适用的类型。

> 比如加法操作对整型，浮点型都一样。区别就是参数类型不一样，而函数体完全一样。

使用的时候指定要适用的类型，也就叫做函数模板的实例化。函数模板实例化时，编译器会将传进来的类型参数替换到函数模板定义的每个位置，从而定义了一个新的函数。

比如我们有一个函数模板：

```c
template <class SomeType>
SomeType sum (SomeType a, SomeType b)
{
  return a+b;
}
```

使用这个模板：

```c
x = sum<int>(10,20);
```

编译器会用`int` 替代每一个SomeType出现的位置。然后就等同于定义了下面一个函数：

```c
int sum (int a, int b)
{
  return a+b;
}
```

上述用传进来的**类型参数**替换**函数模板**的过程就是**函数模板的实例化**(instantiates)。

### 什么是实例化一个函数模板？以及实例化函数模板语法

> Instantiating a template is applying the template to create a function using particular types or values for its template parameters. This is done by calling the *function template*, with the same syntax as calling a regular function, but specifying the template arguments enclosed in angle brackets:
> 
> name <template-arguments> (function-arguments)
> 
> For example, the `sum` function template defined above can be called with:
> 
> `x = sum<int>(10,20);`
> 
> The function `sum<int>` is just one of the possible instantiations of function template `sum`. In this case, by using `int` as template argument in the call, the compiler automatically instantiates a version of `sum` where each occurrence of `SomeType` is replaced by `int`, as if it was defined as:
> 
> ```c
> int sum (int a, int b)
> {
>   return a+b;
> }
> ```

注：定义好了模板之后，我们就可以用这个模板创造东西了。模板的实例化编译器会自动替换`类型参数`来生成指定版本的函数。相当于定义了一个针对传进来的`类型实参`的函数。

---

这里有两个基础概念：1. 模板 2. 模板的实例化。

- 模板就是你定义了一种适用多种类型的函数（有对应定义语法），可以避免代码重复。

- 模板实例化就是在你定义好了模板之后，针对特定类型开始使用这么模板的过程。

上述概念对类模板也适用。因此就不在解释了。思路都是同一种东西对多种类型适用，那么就把类型参数化，编写成模板。使用的时候在给出具体的类型（模板实例化）。

Reference: [Function templates](https://cplusplus.com/doc/tutorial/functions2/)

以上是模板的概念。记住参数化类型就可以了。

# 模板特化(模板专业化)Template Specialization

上述将类型参数化后，定义了模板的通用行为。但如果我们想要针对某种特定类型，有其特殊化的行为如何呢？这就引入了模板专业化。

模板专业化是在已有上述基本模板的前提下，对特定类型进行不同处理。也就是说，没有专业化的类型传入模板就是通用行为，如果传入的是特化的类型，那么就是特化的行为。

> Template specialization is the process of providing explicit implementations for templates to handle specific types differently. It allows us to override the default behavior of a template for a particular type or a subset of types. In C++, we have primary templates, which provide a generic implementation, and specialized templates, which provide custom implementations for specific types.

类模板和函数模板都可以特化（专业化）,下面是函数模板特化：

```c
template <typename T>
T Square(T value) {
    return value * value;
}

// Explicit specialization for 'int'
template <>
int Square<int>(int value) {
    return value * value * value;
}
```

使用时：

```c
 // Explicit specialization for 'int' is used: 5 * 5 * 5 = 125
 int result1 = Square(5); 
 // Generic implementation is used: 3.5 * 3.5 = 12.25 
double result2 = Square(3.5); 
```

使用时语法和通用模板一致，但是对于特定类型，具有特化后的行为。

# 模板特化，模板全特化，模板偏特化

根据特化的参数与通用模板参数的个数关系分为全特化和偏特化。

### 关系

1. **模板特化**是一个总括性的概念，涵盖了为模板参数提供定制实现的所有形式。
2. **模板全特化**是模板特化的一种形式，它为特定类型的模板参数提供完全定制的实现，所有参数都被具体化。
3. **模板偏特化**是模板特化的另一种形式，它为部分模板参数提供定制实现，而不是所有参数都被具体化。

模板特化覆盖通用模板的行为。
