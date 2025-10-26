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

下面是对 `:g` 命令的vim 帮助文档描述，解释的很清楚了。

```vim
2. Multiple repeats              multi-repeat

                                 :g :global E148
:[range]g[lobal]/{pattern}/[cmd]
       Execute the Ex command [cmd] (default ":p") on the
       lines within [range] where {pattern} matches.                                                            

:[range]g[lobal]!/{pattern}/[cmd]
       Execute the Ex command [cmd] (default ":p") on the
       lines within [range] where {pattern} does NOT match.
```

可以在匹配的行上执行一些命令。并且匹配模式可以用 `!` 取反。

#### 6. :saveas 的用法

`:saveas new_file` 命令会将当前的buffer 写入到一个新的文件new_file中，并使得当前的buffer 关联到这个新的文件(即buffer 名字改为new_file)。而原来的文件(old_file)保持未修改之前的状态。之后的修改也是在new_file 的基础上修改。

注：

相当于另存为，原文件内容不变，但是把当前打开的文件也换成另存为后的文件了。

#### 7. vim 高级映射写法解释

inoremap `<silent><expr>` `<C-e>` coc#pum#visible() ? coc#pum#cancel() : "\<C-e>"

inoremap `<silent><expr>` `<C-y>` coc#pum#visible() ? coc#pum#confirm() : "\<C-y>"

上面两个映射的功能是：Use `<C-e>` and `<C-y>` to cancel and confirm completion.

##### Explanation of the mappings

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

#### 8.深刻理解vim 中 :read 和 :write 命令

- vim 中 :read 命令可以从文件中读取数据，还可以读取另一个命令的输出进当前文件。
- vim 中: write {file} 可以将当前文件的选中行，以追加写入或者覆盖写入的方式写进另一个文件{file} 中。

**如：**

- `:read patch` 会把patch 文件的内容添加进当前光标所在行下方。 使用 `:0read patch` 将文件内容添加进文件首部。也可以使用其他数字 `n` 表示插入把patch文件内容插入到第n 行下面。

- `read !{cmd}` 运行{cmd} 命令，并将该命令的输出插入到当前文件光标下面。

- `:.,$write {file}` 把当前行至文件末尾的内容保存进另一个文件{file} 中，如果file 文件已经存在会报错。使用 `:.,$write! {file}` 文件强制覆盖写入。

- `:.write >> {file}` 将当前行以追加的方式写入到文件{file} 中。追加写入不会覆盖掉文件中原内容。

- `:[range]write !{cmd}` 将选中的文本作为标准输入，执行{cmd}命令
  
  官方文档描述：
  
  ```vim
  :[range]w[rite] [++opt] !{cmd}
  
                        Execute {cmd} with [range] lines as standard input
                        (note the space in front of the '!').  {cmd} is
                        executed like with ":!{cmd}", any '!' is replaced with
                        the previous command :!
  ```
  
   如：`3,10w !wc` 使用wc命令统计范围[3，10]行的文本。

#### 9. vim 中强大的过滤命令

过滤命令语法： `!{motion}{program}` 。过滤命令的含义是：选中一些文本块，将其作为程序{program} 的输入，然后将 {program} 的输出替换跳原来的文本块。

比如：假设光标在第一行 `!5Gsort<Enter>`  就是将前五行的文本作为输入，传入sort 函数，然后排序的结果替换原来的前五行。

##### 过滤命令的一般形式是 `:[range]!{program}` program 可以带options

比如 `!5Gsort<Enter>` 会变成 `:.,.+4!sort`

##### 对当前行进行过滤的命令是 `!!{program}`

比如 `!!date` 将当前行的内容替换为当前时间。
