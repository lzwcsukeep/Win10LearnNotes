多态：父类引用指向子类对象

特点：只能访问父类中声明的方法,变量。当子类重写了父类的方法时，访问的是重写后的方法。

原因：因为编译器是根据类型声明调用方法的，因此在调用方法时，是根据父类的类型去找方法，因此也就只能找到父类中已经定义的方法，而看不到子类特有的方法。

![image--multi](C:\Users\lzw\Pictures\多态解释.png)

**子类中重写父类方法的权限修饰符**

特点：子类中重写父类方法的访问修饰符不能变得更加严格，也就是不能缩小父类的访问修饰符

权限修饰符的严格程度：

- private      (最严格，访问范围最小)
- default
- protected
- public

原话：There is Only one rule while doing Method overriding with Access modifiers 

i.e.

```text
If you are overriding any method, overridden method (i.e. declared in subclass) must not be more restrictive.
```

**子类方法不能抛出比父类更大的异常**

> 返回值类型不属于方法签名的一部分，也就是说两个方法只有返回值不同，在同一个类中是不允许的，不叫方法重载。然后在父子类中叫做方法重写。

> 因为返回值类型不参与重写或重载的考虑之中。   --个人思考

**重写之后的方法如果有返回值，还需要保证返回类型的兼容性。**

允许子类将覆盖重写方法的返回类型改为**原返回类型的子类型**。如

```java
public Employee getBuddy(){ ... }
```

经理不会想找底层员工作为工作搭档。为了反映这一点，在子类Manager 中 ，可以如下重写其方法

```java
public Manager getBuddy(){ ... } //OK to change return type
```
