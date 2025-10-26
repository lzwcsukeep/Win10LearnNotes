# shell parameter expansion
shell 脚本里一个很有意思的语法，叫parameter expansion。
> In shell scripting (specifically Bash), the syntax ${1,,} is a parameter expansion feature that converts all characters in the value of $1 to lowercase.

${1,,} 表示把`$1`的所有字符都转成小写。类似于C语言中的 lower() 函数。
类似的还有：
- ${var^^} → uppercase conversion.

- ${var,,} → lowercase conversion.

- ${var~} → toggle case (first char).

- ${var,,[A-Z]} → selectively lowercase only specific characters. 

下面示例中 $1 是 User,USER,uSER 测试表达式 `[ ${1,,} = user ]` 都会返回true。
```shell
 if [ ${1,,} = user ]; then 
 ...
 fi
```
