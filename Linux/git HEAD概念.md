# Detached HEAD state

HEAD 引用的是一个特定的提交，而不是一个有名字的分支，这种情况就是**detached `HEAD` state**

> This is known as being in detached `HEAD` state. It means simply that `HEAD` refers to a specific commit, as opposed to referring to a named branch.
> 
> In fact, we can perform all the normal Git operations.

对于detached HEAD state 我们可以执行所有的正常git 操作。但是如果我们切换回其他分支，在 git 里面基于detached HEAD state 创建的分支由于没有命名分支的索引，会被git 的垃圾搜集器删除掉。

> ```bash
> $ git checkout master
> 
>                HEAD (refers to branch 'master')
>       e---f     |
>      /          v
> a---b---c---d  branch 'master' (refers to commit 'd')
>     ^
>     |
>   tag 'v2.0' (refers to commit 'b')
> ```
> 
> Eventually commit `f` (and by extension commit `e`) will be deleted by the routine Git garbage collection process, unless we create a reference before that happens.If we have not yet moved away from commit `f`, any of these will create a reference to it:
> 
> ```bash
> $ git checkout -b foo  # or "git switch -c foo"  (1)
> $ git branch foo                                 (2)
> $ git tag foo                                    (3)
> ```
