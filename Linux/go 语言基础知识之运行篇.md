# Go 语言运行条件

# 一、一个 `.go` 文件可以直接运行

例如只有一个文件：

### hello.go

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, Go")
}
```

运行：

```bash
go run hello.go
```

输出：

```text
Hello, Go
```

这是最简单的情况。

------

# 二、多个 `.go` 文件一起组成一个程序

例如：

```text
project/
├── main.go
└── add.go
```

### main.go

```go
package main

import "fmt"

func main() {
    fmt.Println(Add(1, 2))
}
```

### add.go

```go
package main

func Add(a, b int) int {
    return a + b
}
```

虽然有两个文件，但它们属于同一个 package：

```go
package main
```

可以运行：

```bash
go run .
```

或者：

```bash
go run main.go add.go
```

输出：

```text
3
```

------

# 三、不是所有 `.go` 文件都能运行

例如：

### math.go

```go
package mathutil

func Add(a, b int) int {
    return a + b
}
```

只有这个文件：

```bash
go run math.go
```

会报错：

```text
package command-line-arguments is not a main package
```

因为：

- package 不是 `main`
- 没有 `main()` 函数

------

# 四、真正能运行的条件

一个 Go 程序必须满足：

### ① 属于 `main` package

```go
package main
```

### ② 有入口函数

```go
func main() {
}
```

满足这两个条件，哪怕只有一个 `.go` 文件，也可以运行。

------

# 五、Go 实际上是按 package 编译，而不是按文件编译

例如：

```text
pgpageshell/
├── main.go
├── page.go
├── parser.go
└── util.go
```

这四个文件：

```go
package main
```

编译时会被视为一个整体：

```bash
go run .
```

编译器会先把：

- main.go
- page.go
- parser.go
- util.go

全部合并成一个 package，然后一起编译。

------

# 六、和 C/C++ 的区别

C++：

```text
main.cpp
util.cpp
```

需要：

```bash
g++ main.cpp util.cpp
```

程序员要显式指定多个文件。

而 Go：

```bash
go run .
```

编译器自动找到当前 package 下所有 `.go` 文件并一起编译。

------

# 七、一句话总结

> **一个 `.go` 文件可以运行，但前提是它属于 `package main` 并且包含 `func main()`。**
>
> 更准确地说，Go 实际上是按 **package** 运行和编译，而不是按单个 `.go` 文件运行和编译。