# What is the Explicit keyword in C++?

explicit 关键字加在具有单个参数的构造函数之前可以避免编译器的隐式转换。

In C++, the explicit keyword is used with a constructor to prevent it from performing implicit conversions. A C++ explicit constructor is marked to not convert types implicitly. This is extremely important as **implicit conversions** often lead to unexpected results. If we have a class that has a constructor with a single argument, the constructor converts the single argument to the **class being constructed** and thus becomes a conversion constructor. To avoid implicit conversions like these, we use the explicit keyword in C++.

## Working of C++ Explicit

Let us first understand what are constructors in C++. In C++, constructors are special methods. They are called automatically whenever we have to create an object of a class. We initialize the data members of the new object with the help of constructors.

Now we shall see the concept of implicit conversions, and how they are avoided with the help of C++ explicit keywords. Implicit means are automatic. It happens without us explicitly specifying the compiler what to do. Casting refers to converting one data type to another. Without the C++ explicit compiler, the compiler can implicitly perform the conversion without us having to cast it.

By adding the keyword explicit before a constructor, we specify the compiler not to perform any implicit conversions on it. We want those constructors to be explicitly called, instead of the compiler performing unwanted type operations. This will get more clear as we see some examples in the **next section**.

## Examples of C++ Explicit

In the below example, we shall see how implicit conversion takes place and how we can avoid it using the C++ explicit keyword.

We create a class demo. The constructor for Demo takes in an integer and assigns it to demo1. There is a get demo() function that returns the value of this integer. We also create another function outside this class which takes in a parameter, an object of the class demo, and prints the value returned by the member function getDemo on that object.

```cpp
// C++ program to illustrate implicit conversions
#include <iostream>
using namespace std;

class Demo{
    public:
        Demo(int n){
            demo1 = n;
        }
        int getDemo(){
            return demo1;
        }
    private:
        int demo1;
};

void getDemoExternally(Demo demo){
    cout << demo.getDemo();
}
// Driver Code
int main()
{
    getDemoExternally(10);
    return 0;
}
```

**Output:**

```
10
```

**Explanation:**

We see that the getDemoExternally function takes in an object of the class demo as a parameter. However we pass an integer to it from our driver code, and it still works fine. This is because the compiler has implicitly converted the integer to the demo class since the demo class has a constructor that takes in an integer. Thus, our program runs successfully.

Let us now see how to prevent the above implicit conversion with the help of the C++ explicit constructor.

To achieve this, we add the **C++ explicit** keyword before the constructor.

```cpp
// C++ program to illustrate implicit conversions
#include <iostream>
using namespace std;

class Demo{
    public:
        explicit Demo(int n){
            demo1 = n;
        }
        int getDemo(){
            return demo1;
        }
    private:
        int demo1;
};

void getDemoExternally(Demo demo){
    cout << demo.getDemo();
}
// Driver Code
int main()
{
    getDemoExternally(10);
    return 0;
}
```

**Output:**

```cpp
prog.CPP: In function ‘int main()’:
prog.cpp:23:23: error: could not convert ‘10’ from ‘int’ to ‘Demo’
   23 |     getDemoExternally(10);
      |                       ^~
      |                       |
      |                       int
```

**Explanation:**

By specifying the C++ explicit keyword before the constructor, we are telling the class to not perform any implicit operations. Thus, the integer 10 is not getting converted to the type Demo class anymore. This is why the compiler throws an error.

ref:[What is explicit in C++? - Scaler Topics](https://www.scaler.com/topics/cpp-explicit/)
