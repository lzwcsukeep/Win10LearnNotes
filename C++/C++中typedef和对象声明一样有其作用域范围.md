#### A `typedef` is scoped exactly as the object declaration would have been, so it can be file scoped or local to a block or (in C++) to a namespace or class.

也就是说typedef 和变量一样遵循作用域规则。  允许不同作用域下不同类型起同样的别名，这时采用哪个实际类型会遵循就近原则，同一作用域下不同类型起相同的别名时会报错(同变量的声明行为一致)。(如有不懂看reference)

### Scope of typedef in C

Good thing is that the typedef follows the scope rules. It means, it can have block scope, file scope, etc. If we want we can use the same name for typedef in different scope. Let us see examples to understand how the typedef in C follows the scope rule.

```c
#include <stdio.h>
#include <stdlib.h>
// define new type for int
typedef int INT;
int AdditionOfTwoNumber(void)
{
    INT a =0,b =0;
    printf("Enter two number\n\n");
    scanf("%d%d",&a,&b);
    return (a+b);
}
int main(int argc, char *argv[])
{
    // define new type for char *
    //Using Same Name INT
    typedef char * INT;
    INT pcMessage = "aticleworld";
    printf("\n\nDisplay Message = %s\n\n",pcMessage);
    printf("Addition of two number = %d\n\n",AdditionOfTwoNumber());
    return 0;
}
```

Output:

```c
[root@localhost learn]# ./a.out 


Display Message = aticleworld

Enter two number

1 2
Addition of two number = 3
```

#### Explanation of the code

We can see that in the code INT is using with typedef for two different types int and char *. We are not getting compiler error because both INT has a different meaning in different scope. Globally INT behaves like int but in the main function, it behaves like the char *.



也就是说typedef 和变量一样遵循作用域规则。

### typedef vs #define

- The typedef has the advantage that it obeys the scope rules that means you can use the same name for the different types in different scopes. typedef can have file scope or block scope in which it is used. In other words, #define does not follow the scope rule.
- typedef is compiler token while #define is a preprocessor token.
- typedef is ended with semicolon while #define does not terminate with a semicolon.
- typedef is used to give a new symbolic name to the existing type while #define is used to create an alias of any type and value.



reference:

[c++ - Please explain syntax rules and scope for &quot;typedef&quot; - Stack Overflow](https://stackoverflow.com/questions/2427739/please-explain-syntax-rules-and-scope-for-typedef)

https://aticleworld.com/typedef-in-c/


