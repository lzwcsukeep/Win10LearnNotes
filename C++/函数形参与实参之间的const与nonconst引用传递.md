函数形参和传递进来的实参都可以有const和non-const 两种形式，当它们同属于const或者同属于non-const的时候好理解。当不同的时候就两种情况。下面讨论。

### 函数形参和实参之间const 和 non-cast 之间赋值 （引用传递的方式）

- 当函数形参是non-const ，传递const给形参  -- 报错。

> 因为通过non-const 引用去修改const变量是未定义行为。

- 当函数形参是const ,传递的实参是non-const -- 可以通过编译，这时候就是const 绑定了non-const,含义就是我不能通过这个引用去修改变量的值。

```cpp
// 这里就是表示不使用引用x来改变传进来的实参的值
void modify_const(const int& x, int& y){
    // x = 40 ;
    y = 40 ;
}
```
