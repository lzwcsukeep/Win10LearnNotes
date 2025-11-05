今天探索了一下vscode的一些很牛逼的功能

可以定制快捷键绑定功能，感觉这个和vim的定制化很像。也不花什么时间。

1. ctrl shift e 可以将焦点放在目录浏览器里，然后浏览选中某个目录，之后ctrl n 创建一个新文件，文件类型可以根据文件后缀确定。

2. 可以打开keybinds.json文件，然后手动的配置指定快捷键的行为。
   每个配置由三部分组成：
   
   - key : 按下的快捷键
   - command ： 快捷键对应的命令
   - when ：快捷键生效的时机，比如当焦点在terminal上才生效。都可以指定的
   
   所以vscode 的定制化功能也很强，感觉不比vim差。
   
   熟练掌握vscode 应该也不错。

重点： vscode 可以定制快捷键的行为。快捷键生效的时机。在keybindings.json文件中。

'''
今天就光鼓捣vscode去了,都没有怎么学习。。。。
2023/5/21
'''

一直困扰自己很久的关于vscode 的三种设置：
User | Remote[ssh:remote host] | Workspace. 今天在vscode 官网上找到了明确描述。

直接看原文：
> VS Code's local User settings are also reused when you are connected to an SSH host. While this keeps your user experience consistent, you may want to vary some of these settings between your local machine and each host. Fortunately, once you have connected to a host, you can also set host-specific settings by running the Preferences: Open Remote Settings command from the Command Palette (F1, Ctrl+Shift+P) or by selecting on the Remote tab in the Settings editor. These will override any User settings you have in place whenever you connect to the host. And Workspace settings will override Remote and User settings.

翻译成中文的理解：
1. 当没有开启远程连接，只在本地进行开发时。
实际的设置只有两种， User 和 workspace ， user 是用户级别的设置， workspace 是当前工作区的设置。

用户级别的设置首先生效，对所有的工作区都一样，当打开某个具体的工作区时，如果用户级别的设置和工作区设置有相同的部分，
那么工作区的该设置会覆盖掉用户级别的设置。
简而言之，用户级别的设置首先生效，如果当前工作区某些设置和用户级别的一样，那么当前工作区的设置覆盖用户级别的设置。

2. 当开启远程连接时，三个级别【用户设置】【远程设置】【工作区设置】都有作用。

vscode 本地用户设置会被复用到远程ssh 主机，当远程 ssh 主机有相同设置项时，远程ssh 项覆盖本地用户设置。
但远程SSH也有本地工作区设置，本地工作区设置会同时覆盖用户设置 和 远程ssh 主机设置。

### 对vscode 的碎碎念

// .vscode settings 的设置结构 都是 key value 的形式
// 前面设置名都是字符串，也叫设置项的id， 识别标志。大概每一个具体的设置
// 在vscode 里面都有唯一的id 标识吧。（很大的可能性）
// 插件的相关设置也在这里面。比如你看这个"C_Cpp.intelliSenseEngine" 
// 还有上面的makfile 的，也是某个插件的设置。
// 综上，如何看待vscode ? 
// 直接把 vscode 当成一个现代编辑器是最合适的。其地位实际上和 vim 很像
// 把vscode 看成现代编辑器，但是可以加上扩展来丰富其功能。对文件的管理，
// 文件的跳转，搜索。这些功能。有些是自带的，有些通过extension来扩展。
// 实际上是非常好用的。和vim很像了，定制化很高。用起来应该也很顺手才对
// 只是你之前一直没有把这些当回事，花大量时间去鼓捣 vim ，如果你把鼓捣vim
// 的时间用来弄vscode ，可能早就很熟悉了。
// vscode 定制化设置和vim 不太一样。 vscode 定制化，是通过json 文件来设置的。
// vim 是通过vimrc。 vscode 的定制化也可以达到很高的地步。
// 你应该把vscode的地位定位和 vim 类似。