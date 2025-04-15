### vim 进阶笔记

#### 1.指定范围替换

可以指定确定的行或行范围进行替换，具体如下

10.3    Command ranges

The ":substitute" command, and many other : commands, can be applied to a
selection of lines.  This is called a range.
   The simple form of a range is {number},{number}.  For example:

        :1,5s/this/that/g

Executes the substitute command on the lines 1 to 5.  Line 5 is included.
The range is always placed before the command.

A single number can be used to address one specific line:

        :54s/President/Fool/

#### 2.寄存器的使用

```textile
"fyas   //拷贝一个句子到f寄存器
"l3Y    //拷贝三个整行到l寄存器
```

现在有了在两个寄存器f ，l  的两段文本，移动到另一个文件(也可以是本文件)想要插入文本的地方：

```textile
"fp  // 将f寄存器的内容粘贴到当前行下方。
```

#### 3.宏录制

1. q+字母({register}) 启动宏录制。结果保存到{register}指定的寄存器中。{register}可以是a-z中任一字母

2. 输入命令

3. 键入q结束宏录制。

录制完毕用"@+字母"命令执行这个宏

#### 4.标记文件位置

1. 命令m+字母。即可标记当前光标下的位置。

2. 使用 **\`+字母** 跳到该字母标记的位置。

3. 使用 **'+字母** 跳到该字母标记的行首的位置。

对于步骤一，使用小写字母标记时只能在当前文件内跳转，使用大写字母标记时，可以在文件之间跳转。即大写字母属于全局标记。小写字母属于局部标记。

#### 5. :g(global) 命令的使用

下面是对`:g` 命令的vim 帮助文档描述，解释的很清楚了。

```bash
2. Multiple repeats              multi-repeat

                                 :g :global E148
:[range]g[lobal]/{pattern}/[cmd]
       Execute the Ex command [cmd] (default ":p") on the
       lines within [range] where {pattern} matches.                                                                  

:[range]g[lobal]!/{pattern}/[cmd]
       Execute the Ex command [cmd] (default ":p") on the
       lines within [range] where {pattern} does NOT match.
```

可以在匹配的行上执行一些命令。并且匹配模式可以用`!` 取反。

#### 6. :saveas {file_name} 的用法。

`:saveas new_file` 命令会将当前的buffer 写入到一个新的文件new_file中，并使得当前的buffer 关联到这个新的文件(即buffer 名字改为new_file)。而原来的文件(old_file)保持未修改之前的状态。之后的修改也是在new_file 的基础上修改。

注：

相当于另存为，原文件内容不变，但是把当前打开的文件也换成另存为后的文件了。

#### 7. vim 高级映射写法解释

inoremap <silent><expr> <C-e> coc#pum#visible() ? coc#pum#cancel() : "\<C-e>"
inoremap <silent><expr> <C-y> coc#pum#visible() ? coc#pum#confirm() : "\<C-y>"

上面两个映射的功能是：Use <C-e> and <C-y> to cancel and confirm completion.

##### Explanation of the mappings:

- `inoremap`: Maps the keys in **insert mode**.
- `<silent>`: Prevents echoing the command.
- `<expr>`: Allows the mapping to evaluate a Vim expression.
- `coc#pum#visible()`: Checks whether the completion menu (popup menu) is visible.
- `coc#pum#cancel()`: Cancels the completion menu.
- `coc#pum#confirm()`: Confirms the selected completion item.
- `"\<C-e>"` / `"\<C-y>"`: Inserts the literal `<C-e>` or `<C-y>` if the menu is not visible.

---

These mappings allow you to customize `<C-e>` and `<C-y>` (Ctrl+e and Ctrl+y) in **insert mode** for use with the **coc.nvim** auto-completion menu:

1. **`<C-e>`**:
   
   - If the **popup menu** is visible (`coc#pum#visible()`), it cancels the completion menu using `coc#pum#cancel()`.
   - Otherwise, it falls back to the default behavior of `<C-e>`.

2. **`<C-y>`**:
   
   - If the **popup menu** is visible (`coc#pum#visible()`), it confirms the current selected completion item using `coc#pum#confirm()`.
   - Otherwise, it falls back to the default behavior of `<C-y>`.

  
