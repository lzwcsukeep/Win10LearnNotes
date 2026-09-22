## Modules, packages, and versions[¶](https://go.dev/ref/mod#modules-overview)

A module is a collection of packages that are released, versioned, and distributed together. Modules may be downloaded directly from version control repositories or from module proxy servers.

A module is identified by a [module path](https://go.dev/ref/mod#glos-module-path), which is declared in a [`go.mod` file](https://go.dev/ref/mod#go-mod-file), together with information about the module’s dependencies. The module root directory is the directory that contains the `go.mod` file. The main module is the module containing the directory where the `go` command is invoked.

Each package within a module is a collection of source files in the same directory that are compiled together. A package path is the module path joined with the subdirectory containing the package (relative to the module root). For example, the module `"golang.org/x/net"` contains a package in the directory `"html"`. That package’s path is `"golang.org/x/net/html"`.

> from ： https://go.dev/ref/mod

---

自主理解： package 是 一组 .go 文件的集合，同一个 package 的文件同时编译。一个目录只能有一个 package，不能两个package 的文件处在同一个目录下。

而  module 又是一组 package 的集合。发布，版本控制是以 module 为单位。



>  非常简要的理解：
>
> - A **module** is a collection of go packages.
> - A **package** is a directory of .go files. Using packages, you organize your code into reusable units.
> - We can add a module to go project or upgrade the module version.



### import 命令导入的是package

[Using the `import` Keyword](https://www.digitalocean.com/community/tutorials/importing-packages-in-go#using-the-import-keyword)

Go uses the `import` keyword to bring package identifiers into the current file. You can import one package per line or use a grouped import block.

Single import:

```go
import "fmt"
```

或者Grouped imports:

```go
import (
    "fmt"
    "os"
)
```

