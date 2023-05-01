### C++ 构造器

C++ 类的 默认构造器

不带参数的构造函数就是默认的构造器，这个构造器特殊在实例化时不需要传递任何参数，连小括号都不要。

> The *default constructor* is the constructor that takes no parameters, and it is special because it is called when an object is declared but is not initialized with any arguments.

如下面代码中的对象`rectb`

```cpp
// overloading class constructors
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle ();
    Rectangle (int,int);
    int area (void) {return (width*height);}
};

Rectangle::Rectangle () {
  width = 5;
  height = 5;
}

Rectangle::Rectangle (int a, int b) {
  width = a;
  height = b;
}

int main () {
  Rectangle rect (3,4);
  Rectangle rectb;
  cout << "rect area: " << rect.area() << endl;
  cout << "rectb area: " << rectb.area() << endl;
  return 0;
}
```

如果一个类没有定义任何构造器，那么编译器就会赠送一个隐含的无参构造器。

> If a class definition has no constructors, the compiler assumes the class to have an implicitly defined *default constructor*. 

但是一旦类中有自己定义的构造器，那么编译器就不再赠送隐式的无参构造器了。因此，如果没有定义无参构造器的话，仅仅用`类名 + 对象名`定义一个对象将会报错。

> But as soon as a class has some constructor taking any number of parameters explicitly declared, the compiler no longer provides an implicit default constructor, and no longer allows the declaration of new objects of that class without arguments. 
