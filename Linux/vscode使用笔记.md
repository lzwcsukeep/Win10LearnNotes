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