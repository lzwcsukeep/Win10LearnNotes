### shell 命令分隔符

shell 命令分割符可以使得shell在一行上写多个命令。重要和神奇的是 `&` 分隔符，以前没有遇到过。稍微总结一下：

> To summarize (non-exhaustively) bash's command operators/separators:
> 
> - `|` pipes (pipelines) the standard output (`stdout`) of one command into the standard input of another one. Note that `stderr` still goes into its default destination, whatever that happen to be.
> - `|&`pipes both `stdout` and `stderr` of one command into the standard input of another one. Very useful, available in bash version 4 and above.
> - `&&` executes the right-hand command of `&&` only if the previous one succeeded.
> - `||` executes the right-hand command of `||` only it the previous one failed.
> - `;` executes the right-hand command of `;` always regardless whether the previous command succeeded or failed. Unless `set -e` was previously invoked, which causes `bash` to fail on an error.
> - `&` executes the left-hand command as a background job, and also concurrently runs the right-hand command (which can also be terminated with `&` to run as a background job).
> 
> 注：`&` 把左边的命令当成后台命令执行，并行执行右边的命令。

详细的解释看官方文档：

https://www.gnu.org/software/bash/manual/bash.html#Lists
