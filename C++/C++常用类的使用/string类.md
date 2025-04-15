string 类的对象表示一个字符串。

构造函数：

![](..\..\img_src\2024-04-24-22-19-15-image.png)

可以空，可以用另一个字符串构造。还可以使用另一个字符串的一部分构造。还可以用C风格的字符串构造。

###### 常用的API

find(str,pos=0) : 查找参数指定的字符串，完全匹配，返回查找的字符串位置，从pos 往后开始找

find_first_of(str,pos=0) ： 找到参数中任意一个就行，返回类型size_t

![](E:\Files\Win10LearnNotes\img_src\Snipaste_2025-04-16_18-03-06.png)

substr(pos ,len) : 返回子串, 从pos 位置起始，延长长度为len 的子串。返回值返回
