git reset 命令完整的用法可以查看[官网描述](https://git-scm.com/docs/git-reset)

这里只记录 git reset 官网描述中第四个用法，且 mode 为  --hard 的笔记。

简言之， 用 `git reset --hard <commit>` 回到先前的某个提交时，当前的工作空间会被更新。并且`git log` 命令也不会显示<commit>后面的提交。 但是，注意但是！！！！在 reset <commit>之后，<commit>未来的提交没有被删除！！！没有被删除！！！！你只要知道它们的hash 值，你仍然可以使用 `git reset --hard <commit_you_want_back>` 回到它们。也就是说在reset 之后的提交还是保存在git 中。

例子：

我的git 仓库中有下面四个提交，使用它们的SHA 值来识别。

```bash
* commit 96e4775f8a5fc4c46eec2333e01dcbd7e7018d80 (HEAD -> master)
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:22:50 2024 +0800
| 
|     fourth com in mas
| 
* commit a47fe30422e8c31321367d6d7d308f4132df4034
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:21:38 2024 +0800
| 
|     third com
| 
* commit 69407843f3a093d4567c230f40ae3d27c1ad5249
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:20:59 2024 +0800
| 
|     sec com
| 
* commit 7931f4854da91de6c5684675e1e8439a46ad4cc4
  Author: lizhiwen <lizhiwen@lejurobot.com>
  Date:   Fri Jan 19 10:20:27 2024 +0800

      first comm
```

当我使用下面命令回到第二个提交，虽然`git log` 看不到第三和第四次提交了，但是它们还在，你还可以使用同样的命令回去。 

```bash
git reset --hard 69407843f3a093d4567c230f40ae3d27c1ad5249  // 第二次提交的hash 值
```

此时使用 git log 输出：

```bash
test$ git log --decorate --all --graph
* commit 69407843f3a093d4567c230f40ae3d27c1ad5249 (HEAD -> master)
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:20:59 2024 +0800
| 
|     sec com
| 
* commit 7931f4854da91de6c5684675e1e8439a46ad4cc4
  Author: lizhiwen <lizhiwen@lejurobot.com>
  Date:   Fri Jan 19 10:20:27 2024 +0800

      first comm
```

使用同样的命令回去第四次提交，reset 后跟的是第四次提交的hash 值：

```bash
test$ git reset --hard 96e4775f8a5fc4c46eec2333e01dcbd7e7018d80 //第四次提交的hash 值
HEAD is now at 96e4775 fourth com in mas
```

再次使用 git log 命令查看：

```bash
test$ git log --decorate --all --graph
* commit 96e4775f8a5fc4c46eec2333e01dcbd7e7018d80 (HEAD -> master)
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:22:50 2024 +0800
| 
|     fourth com in mas
| 
* commit a47fe30422e8c31321367d6d7d308f4132df4034
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:21:38 2024 +0800
| 
|     third com
| 
* commit 69407843f3a093d4567c230f40ae3d27c1ad5249
| Author: lizhiwen <lizhiwen@lejurobot.com>
| Date:   Fri Jan 19 10:20:59 2024 +0800
| 
|     sec com
| 
* commit 7931f4854da91de6c5684675e1e8439a46ad4cc4
  Author: lizhiwen <lizhiwen@lejurobot.com>
  Date:   Fri Jan 19 10:20:27 2024 +0800

      first comm
```

回到了未来。

因此git reset --hard 不会删除提交，它们还在git 仓库中，只要你还记得它们的hash 值，你可以使用git reset --hard 回去。
