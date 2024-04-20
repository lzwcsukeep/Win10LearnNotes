### include_directories 命令

Add include directories to the build.

```cmake
include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])
```

Add the given directories to those the compiler uses to search for include files. Relative paths are interpreted as relative to the current source directory.

> 将给定目录添加到头文件的搜索路径中去。

### add_subdirectory 命令

在 CMake 中，`add_subdirectory` 命令的作用是向当前项目添加一个子目录。这个子目录通常包含另一个独立的 CMake 项目，它可以有自己的 CMakeLists.txt 文件，定义了该子目录中的构建规则和目标。

使用 `add_subdirectory` 可以方便地组织大型项目，使得每个子目录都可以独立构建和测试，同时允许它们在主项目中共享目标和库。

注意： 有自己的CMakeLists.txt 文件，可以独立构建和测试。

https://chat.openai.com/share/a85eb43b-73b8-4b2a-88d2-aed9e3240be4

### find_package 命令

`find_package` 是 CMake 中一个用于查找和载入外部软件包（packages）的命令。它主要用于定位和配置第三方库、工具、框架等，使得 CMake 能够正确地使用它们的头文件、库和其他资源。
并且如果找到该库，会自动定义一些变量。比如头文件包含路径，共享库路径等。

细节看openai链接。

未尽命令：

add_library()

这些命令都可以在官网上找到详细的解释，可以多看一下官网解释和别人的博客。
