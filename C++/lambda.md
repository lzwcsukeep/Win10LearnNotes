### some notes about using lambda expression in c++

![](../img/lambdaexpsyntax.png)

C++ 11 introduced lambda expressions to allow inline functions which can be used for short snippets of code that are not going to be reused and therefore do not require a name.

the form of lambda expression consisting of six parts as following :

```
[capture clause] (parameter list) mutable throw() -> int
{
    definition of method(lambda body)
}
```

1. capture clause (Also known as the lambda-introducer in the C++ specification.)

2. parameter list Optional. (Also known as the lambda declarator)

3. mutable specification Optional.

4. exception-specification Optional.

5. return-type Optional.

6. lambda body.
   
   # Capture clause
   
   A lambda can introduce new variables in its body (in C++14), and it can also access, or capture, variables from the surrounding scope. A lambda begins with the capture clause. It specifies which variables are captured, and whether the capture is by value or by reference. Variables that have the ampersand (&) prefix are accessed by reference and variables that don't have it are accessed by value.

An empty capture clause, [ ], indicates that the body of the lambda expression accesses no variables in the enclosing scope.

You can use a capture-default mode to indicate how to capture any outside variables referenced in the lambda body: [&] means all variables that you refer to are captured by reference, and [=] means they're captured by value. You can use a default capture mode, and then specify the opposite mode explicitly for specific variables. For example, if a lambda body accesses the external variable total by reference and the external variable factor by value, then the following capture clauses are equivalent:

```cpp
[&total, factor]
[factor, &total]
[&, factor]
[=, &total]
```

Only variables that are mentioned in the lambda body are captured when a capture-default is used.

# Parameter list

Lambdas can both capture variables and accept input parameters. A parameter list (lambda declarator in the Standard syntax) is optional and in most aspects resembles the parameter list for a function.

```cpp
auto y = [] (int first, int second)
{
    return first + second;
};
```

# mutable specification Optional.

# exception-specification Optional.

the both are ignored.

# Return type

The return type of a lambda expression is automatically deduced. You don't have to use the auto keyword unless you specify a trailing-return-type. The trailing-return-type resembles the return-type part of an ordinary function or member function. However, the return type must follow the parameter list, and you must include the trailing-return-type keyword -> before the return type.

You can omit the return-type part of a lambda expression if the lambda body contains just one return statement. Or, if the expression doesn't return a value. If the lambda body contains one return statement, the compiler deduces the return type from the type of the return expression. Otherwise, the compiler deduces the return type as void. Consider the following example code snippets that illustrate this principle:

```cpp
auto x1 = [](int i){ return i; }; // OK: return type is int
auto x2 = []{ return{ 1, 2 }; };  // ERROR: return type is void, deducing
                                  // return type from braced-init-list isn't valid
```

# Lambda body

The lambda body of a lambda expression is a compound statement. It can contain anything that's allowed in the body of an ordinary function or member function. The body of both an ordinary function and a lambda expression can access these kinds of variables:

- Captured variables from the enclosing scope, as described previously.

- Parameters.

- Locally declared variables.

- Class data members, when declared inside a class and this is captured.

- Any variable that has static storage duration—for example, global variables.

The following example contains a lambda expression that explicitly captures the variable `n` by value and implicitly captures the variable `m` by reference:  

```cpp
// captures_lambda_expression.cpp
// compile with: /W4 /EHsc
#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   [&, n] (int a) mutable { m = ++n + a; }(4);
   cout << m << endl << n << endl;
}
```

```text
output
5
0
```

Because the variable `n` is captured by value, its value remains 0 after the call to the lambda expression. The mutable specification allows `n` to be modified within the lambda.

# reference link

(https://learn.microsoft.com/en-us/cpp/cpp/lambda-expressions-in-cpp?view=msvc-170)

(https://www.geeksforgeeks.org/lambda-expression-in-c/)

(https://www.cnblogs.com/DswCnblog/p/5629165.html)