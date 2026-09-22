### 使用 -E 选项和 `|` 在  grep 中匹配多个 pattern

使用 -E 选项和 `|` 在  grep 中匹配多个 pattern。

示例：

下面这种使用多个 grep 逐个查看某个 pattern 是否存在可以直接改成更下面的方式：

```bash
lzw@ubuntu:~/workspace/UDB-TX-MYSQL-Work$ git diff lvhui_mysql openhalo --name-only -- src/backend/parser/ | grep analy
src/backend/parser/analyze.c
src/backend/parser/mysql/mys_analyze.c
lzw@ubuntu:~/workspace/UDB-TX-MYSQL-Work$ git diff lvhui_mysql openhalo --name-only -- src/backend/parser/ | grep check
src/backend/parser/check_keywords.pl
lzw@ubuntu:~/workspace/UDB-TX-MYSQL-Work$ git diff lvhui_mysql openhalo --name-only -- src/backend/parser/ | grep gram.y
src/backend/parser/gram.y
lzw@ubuntu:~/workspace/UDB-TX-MYSQL-Work$ git diff lvhui_mysql openhalo --name-only -- src/backend/parser/ | grep scan
src/backend/parser/scan.l
src/backend/parser/scansup.c
```

修改后的：

```bash
lzw@ubuntu:~/workspace/UDB-TX-MYSQL-Work$ git diff lvhui_mysql openhalo --name-only -- src/backend/parser/ | grep -E 'analy|check|gram\.y|scan'
src/backend/parser/analyze.c
src/backend/parser/check_keywords.pl
src/backend/parser/gram.y
src/backend/parser/mysql/mys_analyze.c
src/backend/parser/scan.l
src/backend/parser/scansup.c
```

输出会包含所有匹配这些关键字的文件。

注意：

- `|` 表示 OR。
- `.` 在正则中表示任意字符，所以 `gram.y` 要写成 `gram\.y`。