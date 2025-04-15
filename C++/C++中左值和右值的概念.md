### 什么是左值

左值即lvalue,  是一个有着永久内存位置的对象。

> An lvalue is an object which has its permanent memory location where they are stored (and not temporary objects and literals). We can use the assignment operator to change the value of lvalue objects. Lvalues need not be a variable necessarily

### 什么是右值

右值即rvalue,是一个没有永久性内存地址的**临时对象**或者**字面量**(如10)。

> On the other hand, an rvalue is a temporary object or a literal which doesn't have a permanent memory location. We cannot make assignments to rvalues.

### 左值引用和右值引用

英文名字分别是lvalue reference and rvalue references

**定义左值引用和右值引用**

语法上使用`T&` 定义类型T的左值引用， 使用 `T&&` 定义类型T的右值引用。

```cpp
int a = 10;
int& lref = a;  // lvalue reference

int&& rref = 5 + 3;  // rvalue reference to a temporary
```

绑定规则：

> - lvalue reference 只能绑定左值(lvalues)，除非加了const 修饰(即加了const的话左值引用也可以绑定右值)。
> 
> - rvalue reference 只能接受右值(rvalue).

**When is lvalue reference used?**

- To avoid copying large objects.

- To modify an existing variable.

**When is rvalue reference used?**

- **Move semantics**: To "steal" resources from temporary objects instead of copying.

- **Performance optimization** for big objects like strings or vectors
