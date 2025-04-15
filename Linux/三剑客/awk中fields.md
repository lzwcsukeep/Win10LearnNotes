### 当awk 读取一行记录时会自动将该行记录解析成多段字段

> 这里的record 默认就是文件中的一行(one line)。

When `awk` reads an input record, the record is automatically *parsed* or separated by the `awk` utility into chunks called *fields*. By default, fields are separated by *whitespace*, like words in a line. Whitespace in `awk` means any string of one or more spaces, TABs, or newlines; other characters that are considered whitespace by other languages (such as formfeed, vertical tab, etc.) are *not* considered whitespace by `awk`.

The purpose of fields is to make it more convenient for you to refer to these pieces of the record. You don’t have to use them—you can operate on the whole record if you want—but fields are what make simple `awk` programs so powerful.

You use a dollar sign (`$`) to refer to a field in an `awk` program, followed by the number of the field you want. Thus, `$1` refers to the first field, `$2` to the second, and so on. (Unlike in the Unix shells, the field numbers are not limited to single digits. `$127` is the 127th field in the record.) For example, suppose the following is a line of input:

```t4
This seems like a pretty nice example.
```

Here the first field, or `$1`, is ‘This’, the second field, or `$2`, is ‘seems’, and so on. Note that the last field, `$7`, is ‘example.’. Because there is no space between the ‘e’ and the ‘.’, the period is considered part of the seventh field.

`NF` is a predefined variable whose value is the number of fields in the current record. `awk` automatically updates the value of `NF` each time it reads a record. No matter how many fields there are, the last field in a record can be represented by `$NF`. So, `$NF` is the same as `$7`, which is ‘example.’. If you try to reference a field beyond the last one (such as `$8` when the record has only seven fields), you get the empty string. If used in a numeric operation, you get zero.[22](https://www.gnu.org/software/gawk/manual/html_node/Fields.html#FOOT22)

The use of `$0`, which looks like a reference to the “zeroth” field, is a special case: it represents the whole input record. Use it when you are not interested in specific fields.

---

官方解释：

[Fields (The GNU Awk User&rsquo;s Guide)](https://www.gnu.org/software/gawk/manual/html_node/Fields.html)
