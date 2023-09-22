#### 先根据电路图找到GPIO引脚，然后再根据GPIO符号找到GPIO引脚编号

...



控制gpio引脚

下面sh指令使用GPIO sysfs接口控制gpio引脚，150是上面计算出来的gpio引脚编号

```bash
cd /sys/class/gpio 
echo 150 > export  #使用echo 150 > export将 GPIO 150 导出时，会在/sys/class/gpio目录下创建一个名为gpio150的目录，并在该目录下创建与 GPIO 150 相关的文件。 
cd gpio150
echo out > direction  #（echo out > direction 是将字符串"out"重定向到 GPIO 目录下的 direction 文件中。这是一种使用 sysfs 接口控制 GPIO 引脚方向的方法。
在 sysfs 的 GPIO 接口中，每个 GPIO 引脚目录下都有一个 direction 文件，用于设置 GPIO 引脚的方向。该文件中包含的字符串可以是 "in" 表示输入方向，或者 "out" 表示输出方向。
通过执行 echo out > direction，将 "out" 这个字符串写入 direction 文件中，即可将该 GPIO 引脚设置为输出方向。这意味着你可以通过写入 GPIO 目录下的 value 文件来控制该引脚的输出状态（高或低电平）。
总结一下，echo out > direction 的作用是将 GPIO 引脚设置为输出方向，使你能够通过写入 value 文件来控制该引脚的输出状态）
echo 1 > value # 输出高电平
echo 0 > value # 输出低电平
echo 150 > unexport    #释放资源(即删除/sys/class/gpio/gpio150目录及其下的相关文件。)
```

使用下述命令查看哪些引脚被占用：

```shell
cat /sys/kernel/debug/gpio
```


