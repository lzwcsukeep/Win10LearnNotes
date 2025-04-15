### sed 的 y command 可以进行字符的逐个替换：

> sed 的 y 命令用于逐字符翻译（transliterate）。它类似于 Unix 中的 tr 命令，可以将一组字符逐一替换为另一组字符。

示例 1：字符翻译
假设有一个文件 example.txt，内容如下：hello
如果想把 h 替换为 H，e 替换为 E，o 替换为 O，可以运行：

```bash
sed 'y/heo/HEO/' example.txt
```

输出结果：
HEllO

示例 2：替换数字
假设文件 example.txt 的内容是：12345
如果想将数字 1 替换为 a，2 替换为 b，3 替换为 c：

```bash
sed 'y/123/abc/' example.txt
```

输出结果：
abc45

解释：
语法：y/source/destination/
source：源字符集，表示需要被替换的字符。
destination：目标字符集，表示用来替换的字符。
两者必须长度相同，一一对应进行替换。不支持正则表达式。

注： 记住是逐个翻译，一一对应。

### sed 可以将某个文件的特定行，写入另一个文件

使用 w 命令， `w file_name`

命令：

```bash
sed '/pattern/w file' input_files...
sed '3,5w file' input_files...
```

第一个命令将匹配的模式空间内容写入到文件file 中。

第二个命令将输入文件的3-5行内容写入到文件file 中。

本质都是将模式空间中的内容写入到新文件file中。

#### 示例：

文件example.txt 内容如下:

```textile
line1
line2
line3
others
nonsense
```

命令 `sed '3,5w file' example.txt ` 后，file 文件内容为：

```t4
//file内容
line3
others
nonsense
```

命令 `sed '/line/w file2' example.txt` 后，file2 文件内容为：

```t4
//file2 内容
line1
line2
line3
```
