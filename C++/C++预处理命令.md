## 预处理的宏定义

To define preprocessor macros we can use `#define`. Its syntax is:  

`#define identifier replacement`

example :

```c
#define TABLE_SIZE 100
int table1[TABLE_SIZE];
int table2[TABLE_SIZE];
```

在预处理器将TABLE_SIZE替换掉之后，代码会变成下面这样：

```c
int table1[100];
int table2[100];
```

上面这个很好理解，重点看条件编译。

当然宏定义是单纯的文本替换，当定义函数表达式注意给每个变量加括号，防止出错。

如：

```c
#define getmax(a,b) ((a)>(b)?(a):(b)) 
```

Defined macros are not affected by block structure. A macro lasts until it is undefined with the `#undef` preprocessor directive:(也就是说定义好的宏可以被取消掉)

```c
#define TABLE_SIZE 100
int table1[TABLE_SIZE];
#undef TABLE_SIZE
#define TABLE_SIZE 200
int table2[TABLE_SIZE];
```

This would generate the same code as:

```c
int table1[100];
int table2[200];
```

## 有条件的包含(#ifdef, #ifndef, #if, #endif, #else and #elif)

These directives allow to include or discard part of the code of a program if a certain condition is met.

`#ifdef` allows a section of a program to be compiled only if the macro that is specified as the parameter has been defined, no matter which its value is. For example:

```c
#ifdef TABLE_SIZE
int table[TABLE_SIZE];
#endif
```

In this case, the line of code `int table[TABLE_SIZE];` is only compiled if `TABLE_SIZE` was previously defined with `#define`, independently of its value. If it was not defined, that line will not be included in the program compilation.

`#ifndef` serves for the exact opposite: the code between `#ifndef` and `#endif` directives is only compiled if the specified identifier has not been previously defined. For example:

```c
#ifndef TABLE_SIZE
#define TABLE_SIZE 100
#endif
int table[TABLE_SIZE];
```

In this case, if when arriving at this piece of code, the `TABLE_SIZE` macro has not been defined yet, it would be defined to a value of 100. If it already existed it would keep its previous value since the `#define` directive would not be executed.

注：#ifdef,#ifndef 都需要#endif结尾。中间被包含的代码可以是程序代码也可以是宏定义

> The `#if`, `#else` and `#elif` (i.e., "else if") directives serve to specify some condition to be met in order for the portion of code they surround to be compiled. The condition that follows `#if` or `#elif` can only evaluate constant expressions, including macro expressions. For example:

```c
#if TABLE_SIZE>200
#undef TABLE_SIZE
#define TABLE_SIZE 200

#elif TABLE_SIZE<50
#undef TABLE_SIZE
#define TABLE_SIZE 50

#else
#undef TABLE_SIZE
#define TABLE_SIZE 100
#endif

int table[TABLE_SIZE];
```

Notice how the entire structure of `#if`, `#elif` and `#else` chained directives ends with `#endif`.（very nice example）

注：#if , #elif , #else 是配套的,也要#endif 结尾。

The behavior of `#ifdef` and `#ifndef` can also be achieved by using the special operators `defined` and `!defined` respectively in any `#if` or `#elif` directive:

```c
#if defined ARRAY_SIZE
#define TABLE_SIZE ARRAY_SIZE
#elif !defined BUFFER_SIZE
#define TABLE_SIZE 128
#else
#define TABLE_SIZE BUFFER_SIZE
#endif
```

## Error directive (#error)

This directive aborts the compilation process when it is found, generating a compilation error that can be specified as its parameter:

```c
#ifndef __cplusplus
#error A C++ compiler is required!
#endif 
```

This example aborts the compilation process if the macro name `__cplusplus` is not defined (this macro name is defined by default in all C++ compilers).

## Source file inclusion (#include)

这是最常用的预处理指令了吧

This directive has been used assiduously in other sections of this tutorial. When the preprocessor finds an `#include` directive it replaces it by the entire content of the specified header or file. There are two ways to use `#include`:

```c
#include <header>
#include "file" 
```

In the first case, a *header* is specified between angle-brackets `<>`. This is used to include headers provided by the implementation, such as the headers that compose the standard library (`iostream`, `string`,...). Whether the headers are actually files or exist in some other form is *implementation-defined*, but in any case they shall be properly included with this directive.

The syntax used in the second `#include` uses quotes, and includes a *file*. The *file* is searched for in an *implementation-defined* manner, which generally includes the current path. In the case that the file is not found, the compiler interprets the directive as a *header* inclusion, just as if the quotes (`""`) were replaced by angle-brackets (`<>`).

## Pragma directive (#pragma)

This directive is used to specify diverse options to the compiler. These options are specific for the platform and the compiler you use. Consult the manual or the reference of your compiler for more information on the possible parameters that you can define with `#pragma`.  

If the compiler does not support a specific argument for `#pragma`, it is ignored - no syntax error is generated.    

## Predefined macro names

The following macro names are always defined (they all begin and end with two underscore characters, `_`):

| macro             | value                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|:-----------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `__LINE__`        | Integer value representing the current line in the source code file being compiled.                                                                                                                                                                                                                                                                                                                                                                      |
| `__FILE__`        | A string literal containing the presumed name of the source file being compiled.                                                                                                                                                                                                                                                                                                                                                                         |
| `__DATE__`        | A string literal in the form "Mmm dd yyyy" containing the date in which the compilation process began.                                                                                                                                                                                                                                                                                                                                                   |
| `__TIME__`        | A string literal in the form "hh:mm:ss" containing the time at which the compilation process began.                                                                                                                                                                                                                                                                                                                                                      |
| `__cplusplus`     | An integer value. All C++ compilers have this constant defined to some value. Its value depends on the version of the standard supported by the compiler:<br>- **`199711L`**: ISO C++ 1998/2003<br>- **`201103L`**: ISO C++ 2011<br>Non conforming compilers define this constant as some value at most five digits long. Note that many compilers are not fully conforming and thus will have this constant defined as neither of the values above.<br> |
| `__STDC_HOSTED__` | `1` if the implementation is a *hosted implementation* (with all standard headers available)<br>`0` otherwise.                                                                                                                                                                                                                                                                                                                                           |

The following macros are optionally defined, generally depending on whether a feature is available:

| macro                              | value                                                                                                                                               |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `__STDC__`                         | In C: if defined to `1`, the implementation conforms to the C standard.<br>In C++: Implementation defined.                                          |
| `__STDC_VERSION__`                 | In C:<br>- **`199401L`**: ISO C 1990, Ammendment 1<br>- **`199901L`**: ISO C 1999<br>- **`201112L`**: ISO C 2011<br>In C++: Implementation defined. |
| `__STDC_MB_MIGHT_NEQ_WC__`         | `1` if multibyte encoding might give a character a different value in character literals                                                            |
| `__STDC_ISO_10646__`               | A value in the form `yyyymmL`, specifying the date of the Unicode standard followed by the encoding of `wchar_t` characters                         |
| `__STDCPP_STRICT_POINTER_SAFETY__` | `1` if the implementation has *strict pointer safety* (see `[get_pointer_safety](https://cplusplus.com/get_pointer_safety)`)                        |
| `__STDCPP_THREADS__`               | `1` if the program can have more than one thread                                                                                                    |

Particular implementations may define additional constants.

For example:

```c
// standard macro names
#include <iostream>
using namespace std;

int main()
{
  cout << "This is the line number " << __LINE__;
  cout << " of file " << __FILE__ << ".\n";
  cout << "Its compilation began " << __DATE__;
  cout << " at " << __TIME__ << ".\n";
  cout << "The compiler gives a __cplusplus value of " << __cplusplus;
  return 0;
}
```

output:

```c
[root@localhost learn]# ./a.out 
This is the line number 7 of file preprocessor.cpp.
Its compilation began May 26 2023 at 01:23:50.
The compiler gives a __cplusplus value of 199711
```
