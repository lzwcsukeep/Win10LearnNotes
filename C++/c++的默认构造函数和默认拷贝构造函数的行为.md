## 默认构造函数(default constructor)

##### 什么是默认构造函数？

> **不带任何参数或者带了参数但是每个参数都有默认值的构造函数。**
> 
> 在C++ 标准中将默认构造函数描述为**在调用时不需要提供实参的构造函数**。这包括有形参但是形参都有默认值的构造函数。
> 
> In other languages (e.g. in C++) it is a constructor that can be called without having to provide any arguments, irrespective of whether the constructor is auto-generated or user-defined.
> 
> If the constructor does have one or more parameters, but they all have default values, then it is still a default constructor.
> 
> Remember that each class can have at most one default constructor, either one without parameters, or one whose all parameters have default values

下面是不带任何参数的默认构造函数的例子：

```cpp
class MyClass
{
public:
   /* 不带参数的默认构造函数 */
   MyClass();  // constructor declared

private:
    int x;
};

MyClass::MyClass() : x(100)  // constructor defined
{
}

int main()
{
    MyClass m;  // at runtime, object m is created, and the default constructor is called
}
```

下面是有参数但是每个参数都有默认值的默认构造函数例子：

```cpp
class MyClass
{
public:                
   /*参数带有默认值，该构造函数仍然叫默认构造，一个类只能有一个默认构造函数*/
   MyClass (int i = 0, std::string s = "");  // constructor declared

private:
    int x;
    int y;
    std::string z;
};

MyClass::MyClass(int i, std::string s)     // constructor defined
{
    x = 100;
    y = i;
    z = s;
}
```

##### 什么时侯编译器会给你自动生成一个默认构造函数，什么时候不会？

> 当你定义了任何其他形式的构造函数时，编译器不会自动给你生成默认构造函数。
> 
> 如果还需要默认构造函数，得自己定义。
> 
> 当你没有定义任何形式的构造函数时，编译器会自动给你生成一个默认构造函数，这个自动生成的默认构造函数相当于显式定义一个函数体为空的默认构造函数。

注意：

If constructors are explicitly defined for a class, but they are all non-default, the compiler will not implicitly define a default constructor, leading to a situation where the class does not have a default constructor.

On the other hand in C++11 a default constructor can be explicitly created:

```cpp
class MyClass
{
public:
    MyClass () = default;  // force generation of a default constructor
};
```

Or explicitly inhibited:

```cpp
class MyClass
{
public:
    MyClass () = delete;  // prevent generation of default constructor
};
```

##### 默认构造函数什么时候会被调用？

- When an object value is declared with no argument list (e.g.: `MyClass x;`) or allocated dynamically with no argument list (e.g.: `new MyClass;` or `new MyClass();`), the default constructor of `MyClass` is used to initialize the object.
- When an array of objects is declared, e.g. `MyClass x[10];`; or allocated dynamically, e.g. `new MyClass [10]`. The default constructor of `MyClass` is used to initialize all the elements.
- When a derived class constructor does not explicitly call the base class constructor in its initializer list, the default constructor for the base class is called.
- When a class constructor does not explicitly call the constructor of one of its object-valued fields in its initializer list, the default constructor for the field's class is called.
- In the standard library, certain containers "fill in" values using the default constructor when the value is not given explicitly. E.g. `vector<MyClass>(10);` initializes the vector with ten elements, which are filled with a default-constructed `MyClass` object.

上述场景中默认构造函数很重要，如果类缺少默认构造函数，将会是一个错误。

### Trivial default constructor（无价值的默认构造函数）

The default constructor for class `T` is trivial (i.e. performs no action) if all of the following is true:

- The constructor is not user-provided (i.e., is implicitly-defined or defaulted on its first declaration)
- `T` has no virtual member functions
- `T` has no virtual base classes

| - `T` has no non-static members with [default initializers](https://en.cppreference.com/w/cpp/language/data_members#Member_initialization "cpp/language/data members"). | (since C++11) |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |

- Every direct base of `T` has a trivial default constructor
- Every non-static member of class type (or array thereof) has a trivial default constructor

**A trivial default constructor is a constructor that performs no action. All data types compatible with the C language (POD types) are trivially default-constructible.**

##### 编译器隐式提供的默认构造函数会干什么？

Implicitly-defined default constructor

If the implicitly-declared default constructor is not defined as deleted, it is defined (that is, a function body is generated and compiled) by the compiler if [odr-used](https://en.cppreference.com/w/cpp/language/definition#ODR-use "cpp/language/definition") or [needed for constant evaluation](https://en.cppreference.com/w/cpp/language/constant_expression#Functions_and_variables_needed_for_constant_evaluation "cpp/language/constant expression") (since C++11), and it has the same effect as a user-defined constructor with empty body and empty initializer list. **That is, it calls the default constructors of the bases and of the non-static members of this class. Class types with an empty user-provided constructor may get treated differently than those with an implicitly-defined or defaulted default constructor during [value initialization](https://en.cppreference.com/w/cpp/language/value_initialization "cpp/language/value initialization").**

也就是说编译器隐式提供的默认构造函数，不一定是什么都不做，有可能会调用基类的构造函数或非静态成员的构造函数。也就是说不一定是trivial default constructor。所有和C 语言兼容的自定义类型的默认构造函数是trivial default constructor。 

---

<-------------------------------------------------------分隔线---------------------------------------------------------->

## 默认拷贝构造函数

#### 什么是默认拷贝构造？

A default copy constructor is a copy constructor created by the compiler by default.

When a copy constructor is not defined, the C++ compiler automatically supplies with its self-generated constructor that copies the values of the object to the new object.

默认拷贝构造是由编译器隐式提供的一个拷贝构造函数

当没有用户定义的拷贝构造函数时，C++的编译器就会自动提供一个将一个对象的值拷贝给另一个同类型的新对象的构造函数。

#### 默认拷贝构造都做了什么？

The default copy constructor will do a bit-wise copy if the object is known to be trivially copyable, or a member-wise copy if not. However, the compiler does not always have enough information to perform a proper copy of each member. For example, a pointer is copied by making a copy of the pointer, not by making a copy of the pointed object. That's why you should generally not rely on the compiler providing you with a default copy constructor when your object is not trivially copyable.

**如果已知对象是可琐碎拷贝的，默认的拷贝构造函数将做按位（bit-wise copy）的拷贝** ，如果不是，则做按成员(Member-wise Copy)的拷贝。然而，编译器并不总是有足够的信息来对每个成员进行适当的拷贝。例如，指针的复制是通过对指针的复制，而不是通过对被指对象的复制。**这就是为什么当你的对象不能被琐碎地拷贝时，你一般不应该依赖编译器为你提供的默认拷贝构造函数。(也就是说得自己写拷贝构造函数，明确拷贝时的行为，不然可能会出Bug)**

#### 无价值的拷贝构造(Trivial copy constructor)

<u>The copy constructor for class `T` is trivial if all of the following are true:</u>

- it is not user-provided (that is, it is implicitly-defined or defaulted);
- `T` has no virtual member functions;
- `T` has no virtual base classes;
- the copy constructor selected for every direct base of `T` is trivial;
- the copy constructor selected for every non-static class type (or array of class type) member of `T` is trivial;

A trivial copy constructor for a non-union class effectively copies every scalar subobject (including, recursively, subobject of subobjects and so forth) of the argument and performs no other action. However, padding bytes need not be copied, and even the object representations of the copied subobjects need not be the same as long as their values are identical.

[TriviallyCopyable](https://en.cppreference.com/w/cpp/named_req/TriviallyCopyable "cpp/named req/TriviallyCopyable") objects can be copied by copying their object representations manually, e.g. with [std::memmove](https://en.cppreference.com/w/cpp/string/byte/memmove "cpp/string/byte/memmove"). **All data types compatible with the C language (POD types) are trivially copyable.**

补充：

##### 什么是拷贝构造函数？

当构造函数的形参是自身类型的对象时，此时的构造函数就是拷贝构造。也就是用一个对象去初始化同类型的另一个对象。

拷贝构造函数可以分两类：

- 自定义的拷贝构造函数

- 默认拷贝构造函数

#### 拷贝构造函数调用的时机

The copy constructor is called whenever an object is [initialized](https://en.cppreference.com/w/cpp/language/initialization "cpp/language/initialization") (by [direct-initialization](https://en.cppreference.com/w/cpp/language/direct_initialization "cpp/language/direct initialization") or [copy-initialization](https://en.cppreference.com/w/cpp/language/copy_initialization "cpp/language/copy initialization")) from another object of the same type (unless [overload resolution](https://en.cppreference.com/w/cpp/language/overload_resolution "cpp/language/overload resolution") selects a better match or the call is [elided](https://en.cppreference.com/w/cpp/language/copy_elision "cpp/language/copy elision")), which includes

- initialization: T a = b; or T a(b);, where b is of type `T`;
- function argument passing: f(a);, where a is of type `T` and f is void f(T t);
- function return: return a; inside a function such as T f(), where a is of type `T`, which has no [move constructor](https://en.cppreference.com/w/cpp/language/move_constructor "cpp/language/move constructor").

总结：

如果对象是可以无价值的拷贝，那么默认的拷贝构造函数会进行bit-wise拷贝（浅拷贝）。

如果对象不是无价值的拷贝，那么默认的拷贝构造函数会进行member-wise 拷贝(深拷贝)。但是常常这种时候不能依赖编译器提供的默认拷贝函数，而是需要自己定义拷贝构造，明确拷贝时的行为。
