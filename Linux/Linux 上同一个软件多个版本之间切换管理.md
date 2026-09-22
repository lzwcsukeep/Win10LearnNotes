本文以 `clangd` 为例。

系统里同时存在多个 clangd 版本时，不带后缀的 `clangd` 命令默认还指向旧版本，这是正常现象。要让 21 版本生效，需要主动“告诉”系统。

### 方式一：命令行全局切换（推荐）

在终端里直接敲 `clangd` 就能用 21 版本，这需要借助 `update-alternatives` 来管理。

1.  **先确认 clangd-21 已装好**：`ls /usr/bin/clangd-21`，如果有输出就说明安装成功了。
2.  **注册 clangd-21 作为可选版本**：把 21 版本加入 alternatives 系统，优先级数字（比如 100）越大，在自动模式下越优先。
    
    ```bash
    sudo update-alternatives --install /usr/bin/clangd clangd /usr/bin/clangd-21 100
    ```
3.  **手动选择默认版本**：运行下面的命令，在交互界面里选 clangd-21 对应的编号。
    
    ```bash
    sudo update-alternatives --config clangd
    ```
4.  **验证**：再次运行 `clangd --version`，应该就会显示 21.1.8 了。

### 方式二：在编辑器中单独指定

如果你只在 VS Code 或其他编辑器里用 clangd，不想动系统全局设置，可以在编辑器插件设置里直接指定路径。

*   **找到设置项**：在 clangd 插件的设置里，找到类似 `Clangd: Path` 或 `clangd.path` 的选项。
*   **填入路径**：将其值设置为 `/usr/bin/clangd-21`。这样编辑器就会调用 21 版本，而终端里的 `clangd` 命令不受影响。

### 方式三：临时使用

如果只是偶尔想在终端里用 21 版本，直接输入带版本号的命令即可：
```bash
clangd-21 --version
```
这不会改变任何默认设置，只是临时调用一次。