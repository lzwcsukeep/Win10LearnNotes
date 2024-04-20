参考链接 ：https://blog.csdn.net/weixin_41580036/article/details/124427633

执行命令 `tput colors` ， 输出 8 ，则说明xshell 目前是 8位色彩。需要设置成256 位色彩。

设置如下：
在 bashrc 文件中添加下面语句
```bash
if [ -e /usr/share/terminfo/x/xterm-256color ]; then
    export TERM='xterm-256color'
else
    export TERM='xterm-color'
fi
```

然后在vimrc 文件中添加如下配置：

`set t_Co=256`


bash 下执行命令 `tput colors` ，如果输出 256 则设置成功，可以尽情使用vim 下面的配色方案了。