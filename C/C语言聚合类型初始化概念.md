# 聚合类型

聚合类型，在写C 语言的时候其实经常再用，但很少提及这个概念，也很少精确定义什么是聚合类型，那么什么是聚合类型呢？

**聚合类型（Aggregate Type）** 是 C 语言中的一个术语，指的是**由多个成员组合而成的类型**，可以使用花括号 `{}` 一次性初始化。

C 标准中的聚合类型主要包括：

1. **数组（Array）**
2. **结构体（Struct）**

> 注意：**联合体（union）虽然也可以用 `{}` 初始化，但严格来说在 C 标准中并不属于 Aggregate Type**，而是另一类对象（不同版本标准表述略有区别）。很多资料会把它们放在一起介绍，因为初始化语法很相似。

------

## 1. 数组

例如

```c
int a[3] = {1, 2, 3};
```

数组由多个 `int` 元素组成，因此它是聚合类型。

内存布局：

```
a
+---+---+---+
| 1 | 2 | 3 |
+---+---+---+
```

------

## 2. 结构体

例如

```c
struct Person
{
    char *name;
    int age;
};

struct Person p = {"Tom", 18};
```

结构体由多个不同类型的成员组成，因此也是聚合类型。

内存布局：

```
Person
+---------+
| name    |
+---------+
| age     |
+---------+
```

------

## 3. 嵌套聚合类型

聚合类型可以包含聚合类型。

例如：

```c
struct Address
{
    char *city;
    int zip;
};

struct Person
{
    char *name;
    struct Address addr;
};

struct Person p =
{
    "Tom",
    {
        "Beijing",
        100000
    }
};
```

这里：

```
Person
├── name
└── addr
      ├── city
      └── zip
```

初始化时就是一层 `{}` 对应一层聚合对象。

------

## PostgreSQL 中的例子

你的例子就是典型的**聚合嵌套聚合**：

```c
struct config_bool ConfigureNamesBool[] =
{
    {
        {"enable_seqscan", ...},
        &enable_seqscan,
        true,
        NULL,
        NULL,
        NULL
    }
};
```

可以理解成：

```
数组 ConfigureNamesBool
│
└── 第0个元素（struct config_bool）
        │
        ├── gen（struct config_generic）
        │      ├── name
        │      ├── context
        │      ├── group
        │      └── ...
        │
        ├── variable
        ├── boot_val
        ├── check_hook
        ├── assign_hook
        └── show_hook
```

因此会出现两层花括号：

- 第一层 `{}`：初始化数组元素（`struct config_bool`）。
- 第二层 `{}`：初始化其中的成员 `gen`（`struct config_generic`）。

------

## 为什么叫“聚合（Aggregate）”？

因为它表示**把多个成员聚合（aggregate）成一个对象**。

例如：

```c
struct Point
{
    int x;
    int y;
};
```

`Point` 并不是一个单独的数据，而是把两个 `int` 聚合成了一个新的类型。

同样：

```c
int a[5];
```

数组也是把 5 个 `int` 聚合成了一个整体。

------

## 哪些不是聚合类型？

普通的标量（Scalar）类型不是聚合类型，例如：

```c
int x = 10;
double d = 3.14;
char c = 'A';
int *p = NULL;
```

这些都是单个值，没有内部成员，因此不能写成：

```c
int x = {1, 2};      // 错误
```

------

## 总结

| 类型     | 是否聚合类型                                       | 是否可用 `{}` 初始化                                 |
| -------- | -------------------------------------------------- | ---------------------------------------------------- |
| `int`    | ❌ 否                                               | 可以写 `{1}`，但它本质上是标量初始化，不是聚合初始化 |
| `double` | ❌ 否                                               | 可以写 `{3.14}`                                      |
| 指针     | ❌ 否                                               | 可以写 `{NULL}`                                      |
| 数组     | ✅ 是                                               | `int a[] = {1,2,3};`                                 |
| `struct` | ✅ 是                                               | `struct P p = {"Tom",18};`                           |
| `union`  | ⚠️ 可用 `{}` 初始化，但严格来说通常不归类为聚合类型 | `union U u = {1};`                                   |

理解 PostgreSQL 源码时，可以把**聚合类型**简单理解为：**内部还有多个成员，需要用花括号按层次初始化的类型**。这也是为什么 GUC 定义中会看到一层又一层 `{}` 的原因。

# 聚合类型位置初始化

聚合类型初始化时有一定的规则，最常见的就是位置初始化。位置初始化必须严格按照成员的声明顺序进行。不能跳过。但是最后面的成员可以省略，省略的成员置为0。这是一条C 语言中非常简单，但是非常重要的一条规则。

**传统的聚合类型初始化（位置初始化，positional initialization）必须严格按照成员声明的顺序进行**。这是 C 语言中非常重要的一条规则。

例如：

```c
struct Person
{
    char *name;
    int age;
    double salary;
};
```

成员顺序是：

```text
name
↓
age
↓
salary
```

那么初始化时必须按这个顺序：

```c
struct Person p =
{
    "Tom",
    18,
    10000.0
};
```

编译器会按位置一一对应：

```text
"Tom"    -> name
18       -> age
10000.0  -> salary
```

------

## 如果顺序写错了

例如：

```c
struct Person p =
{
    18,
    "Tom",
    10000.0
};
```

那么编译器会理解为：

```text
18       -> name      // 错误
"Tom"    -> age       // 错误
10000.0  -> salary
```

通常会报类型不匹配的警告或错误，例如：

```
warning: initialization of 'char *' from 'int'
warning: initialization of 'int' from 'char *'
```

------

## 可以省略后面的成员，但不能跳过中间成员

例如：

```c
struct Person
{
    char *name;
    int age;
    double salary;
};
```

下面是合法的：

```c
struct Person p =
{
    "Tom",
    18
};
```

等价于：

```c
struct Person p =
{
    "Tom",
    18,
    0.0
};
```

未提供初始化值的成员会自动初始化为 **0**（或 `NULL`）。

但是不能写：

```c
struct Person p =
{
    "Tom",
    10000.0
};
```

你可能想表达：

```text
name = "Tom"
salary = 10000.0
```

但编译器会认为：

```text
name = "Tom"
age = 10000.0      // 转换为 int
salary = 0.0
```

因为第二个位置对应的是 `age`。

------

## PostgreSQL 中就是按顺序初始化的

你的例子：

```c
struct config_bool
{
    struct config_generic gen;
    bool *variable;
    bool boot_val;
    GucBoolCheckHook check_hook;
    GucBoolAssignHook assign_hook;
    GucShowHook show_hook;
    bool reset_val;
    void *reset_extra;
};
```

初始化：

```c
{
    {"enable_seqscan", ...},   // gen
    &enable_seqscan,           // variable
    true,                      // boot_val
    NULL,                      // check_hook
    NULL,                      // assign_hook
    NULL                       // show_hook
}
```

对应关系就是：

```text
第1项 -> gen
第2项 -> variable
第3项 -> boot_val
第4项 -> check_hook
第5项 -> assign_hook
第6项 -> show_hook
```

后面的两个成员：

```c
bool reset_val;
void *reset_extra;
```

没有提供初始化值，因此自动初始化为：

```c
reset_val = false;
reset_extra = NULL;
```

------

## 如何避免依赖成员顺序？

从 **C99** 开始，可以使用**指定成员初始化（designated initializer）**：

```c
struct Person p =
{
    .salary = 10000.0,
    .name = "Tom",
    .age = 18
};
```

特点：

- 不需要按成员声明顺序写。
- 只初始化需要的成员，其余成员自动置零。
- 即使以后结构体增加了成员，代码也不容易出错。

例如：

```c
struct Person p =
{
    .name = "Tom"
};
```

等价于：

```c
p.name = "Tom";
p.age = 0;
p.salary = 0.0;
```

------

## 总结

对于**位置初始化**（PostgreSQL 源码大量使用的方式）：

1. **必须按成员声明顺序初始化。**
2. **可以省略最后若干个成员**，它们会自动初始化为 0 或 `NULL`。
3. **不能跳过中间成员**，否则后面的值会按位置匹配到错误的成员。
4. **嵌套结构体也是同样的规则**，每一层都按该结构体自己的成员顺序进行初始化。

因此，在阅读 PostgreSQL 源码时，一个很实用的方法是：**一边看结构体定义，一边从上到下对照初始化列表**，这样每个值对应哪个成员就会非常清楚。