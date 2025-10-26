### exec族系列函数原型

NAME 

execl, execlp, execle, execv, execvp, execvpe - execute a file

SYNOPSIS

```c
       #include <unistd.h>

       extern char **environ;

       int execl(const char *pathname, const char *arg, ...
                       /* (char  *) NULL */);
       int execlp(const char *file, const char *arg, ...
                       /* (char  *) NULL */);
       int execle(const char *pathname, const char *arg, ...
                       /*, (char *) NULL, char *const envp[] */);
       int execv(const char *pathname, char *const argv[]);
       int execvp(const char *file, char *const argv[]);
       int execvpe(const char *file, char *const argv[],
                       char *const envp[]);
```

### 如何区分exec族系列函数

`exec` 系列函数名称中的 `l`、`v`、`p`、`e` 是后缀标识，分别代表不同的参数传递方式、路径搜索行为和环境变量处理方式。理解这些后缀的含义，就能快速区分各个函数的功能差异。

### 各后缀的含义

#### 1. `l`（list：参数列表）

- **含义**：表示函数通过**可变参数列表**（逐个参数）传递命令行参数。

- **特点**：参数需要逐个列出，最后必须以 `(char*)NULL` 作为结束标志。

- **示例**：`execl("/bin/ls", "ls", "-l", "-a", (char*)NULL)`
  
  这里，`"ls"` 是程序名（通常与路径中的文件名一致），`"-l"`、`"-a"` 是后续参数，最后用 `NULL` 结束。

#### 2. `v`（vector：参数数组）

- **含义**：表示函数通过**字符串数组（指针数组）** 传递命令行参数。

- **特点**：参数被封装在一个 `char*[]` 类型的数组中，数组最后一个元素必须是 `NULL`。

- **示例**：
  
  ```c
  char *argv[] = {"ls", "-l", "-a", NULL};
  execv("/bin/ls", argv);
  ```

#### 3. `p`（path：路径搜索）

- **含义**：表示函数会**自动搜索 `PATH` 环境变量**中的目录，无需指定可执行文件的完整路径（除非文件名包含 `/`）。

- **特点**：若传入的文件名不含 `/`（如 `"ls"`），函数会在 `PATH` 目录中查找该文件；若含 `/`（如 `"./a.out"`），则直接按路径执行。

- **示例**：`execlp("ls", "ls", "-l", (char*)NULL)`
  
  无需写 `/bin/ls`，函数会自动在 `PATH` 中找到 `ls` 并执行。

#### 4. `e`（environment：环境变量）

- **含义**：表示函数允许**自定义环境变量**，而非使用当前进程的 `environ` 环境变量。

- **特点**：函数参数中包含一个额外的环境变量数组（`char*[]` 类型），数组最后以 `NULL` 结束。

- **示例**：
  
  ```c
  char *envp[] = {"PATH=/usr/local/bin", "LANG=en_US.UTF-8", NULL};
  execle("/bin/ls", "ls", "-l", (char*)NULL, envp);
  ```
  
  这里 `ls` 进程会使用自定义的 `envp` 环境变量，而非继承当前进程的环境。

### 函数与后缀的对应关系

| 函数名       | 包含后缀        | 含义（参数 + 路径 + 环境）                 |
| --------- | ----------- | -------------------------------- |
| `execl`   | `l`         | 用参数列表传递参数，需指定完整路径，使用当前进程环境变量     |
| `execlp`  | `l`+`p`     | 用参数列表传递参数，自动搜索 `PATH`，使用当前进程环境变量 |
| `execle`  | `l`+`e`     | 用参数列表传递参数，需指定完整路径，使用自定义环境变量      |
| `execv`   | `v`         | 用参数数组传递参数，需指定完整路径，使用当前进程环境变量     |
| `execvp`  | `v`+`p`     | 用参数数组传递参数，自动搜索 `PATH`，使用当前进程环境变量 |
| `execvpe` | `v`+`p`+`e` | 用参数数组传递参数，自动搜索 `PATH`，使用自定义环境变量  |

### 总结

- `l`/`v`：区分参数传递方式（列表 / 数组）。
- `p`：是否自动搜索 `PATH`（有 `p` 则搜索，无 `p` 需完整路径）。
- `e`：是否自定义环境变量（有 `e` 则用传入的环境数组，无 `e` 则继承当前环境）。

根据实际需求组合这些特性，即可选择合适的 `exec` 函数（例如：需要动态参数 + 自动搜路径 → 用 `execvp`）。
