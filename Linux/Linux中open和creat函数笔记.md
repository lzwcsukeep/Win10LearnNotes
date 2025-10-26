### 函数原型

       #include <sys/types.h>
       #include <sys/stat.h>
       #include <fcntl.h>

       int open(const char *pathname, int flags);
       int open(const char *pathname, int flags, mode_t mode);

       int creat(const char *pathname, mode_t mode);

### mode 实参指定了新创建的文件的权限bits
from man open

>The mode argument specifies the file mode bits be applied when a new file is created.This argument must be supplied when O_CREAT or O_TMPFILE is specified in flags; if neither O_CREAT nor O_TMPFILE is specified, then mode is ignored.

重点： This argument must be supplied when O_CREAT or O_TMPFILE is specified in flags
当open 函数的flags 参数包含O_CREAT 时，必须提供 mode 参数。

