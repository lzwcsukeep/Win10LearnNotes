# 解决 git-bash 中文乱码问题
[解决问题的链接] : https://www.cnblogs.com/sdlz/p/13023342.html

一个命令解决问题：

```bash
git config --global core.quotepath false
```

简单描述：

原因：
在默认设置下，中文文件名在工作区状态输出，中文名不能正确显示，而是显示为八进制的字符编码。

解决办法：
将git 配置文件 `core.quotepath`项设置为`false`。
quotepath表示引用路径
加上--global表示全局配置

git bash 终端输入命令：
```bash
git config --global core.quotepath false
```

以上就可以解决了。链接里还有其他设置。