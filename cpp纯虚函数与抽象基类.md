#### 纯虚函数(pure virtual functions)

不带定义的虚成员函数。

> virtual member functions without definition (known as pure virtual functions)

纯虚函数定义格式： 去掉函数体，在函数声明的圆括号和分号之间加上=0。

如下面的`area()` 函数：

```cpp
// abstract class CPolygon
class Polygon {
  protected:
    int width, height;
  public:
    void set_values (int a, int b)
      { width=a; height=b; }
    virtual int area () =0;
};
```

#### 含有至少一个纯虚函数的类叫做抽象基类（Abstract base classes）

>  Classes that contain at least one *pure virtual function* are known as *abstract base classes*.

抽象基类不能直接实例化对象。但是可以定义抽象基类的指针，并以多态的形式指向子类的对象。

[Polymorphism - C++ Tutorials](https://www.cplusplus.com/doc/tutorial/polymorphism/)
