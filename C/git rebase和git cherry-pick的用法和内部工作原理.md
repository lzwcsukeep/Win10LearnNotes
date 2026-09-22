## git rebase

Reapply commits on top of another base tip

### DESCRIPTION

Transplant a series of commits onto a different starting point. You can also use `git` `rebase` to reorder or combine commits: see INTERACTIVE MODE below for how to do that.

For example, imagine that you have been working on the `topic` branch in this history, and you want to "catch up" to the work done on the `master` branch.

```
          A---B---C topic
         /
    D---E---F---G master
```

You want to transplant the commits you made on `topic` since it diverged from `master` (i.e. A, B, and C), on top of the current `master`. You can do this by running `git` `rebase` `master` while the `topic` branch is checked out. If you want to rebase `topic` while on another branch, `git` `rebase` `master` `topic` is a shortcut for *git checkout topic && git rebase master*.

```
                  A'--B'--C' topic
                 /
    D---E---F---G master
```

If there is a merge conflict during this process, `git` `rebase` will stop at the first problematic commit and leave conflict markers. If this happens, you can do one of these things:

1. Resolve the conflict. You can use `git` `diff` to find the markers (<<<<<<) and make edits to resolve the conflict. For each file you edit, you need to tell Git that the conflict has been resolved. You can mark the conflict as resolved with `git` `add` *<filename>*. After resolving all of the conflicts, you can continue the rebasing process with

   ```bash
   git rebase --continue
   ```

2. Stop the `git` `rebase` and return your branch to its original state with

   ```bash
   git rebase --abort
   ```

3. Skip the commit that caused the merge conflict with

   ```bash
   git rebase --skip
   ```

### 内部工作原理

当当前分支处在topic 上，执行 `git rebase master` 的内部工作流程大概是：

1. 首先找到 master 和 topic 开始分叉的提交 E
2. 然后记录E 到 topic 的 每个提交的修改diff，临时保存下来。比如 A',B',C'
3. 以 master 的最新提交G 为 基准，逐个应用 A',B',C' ，每个修改的应用都会生成一个新的提交
4. 有冲突的话处理冲突

Official Ref: https://git-scm.com/docs/git-rebase

Other Ref: https://waynerv.com/posts/git-rebase-intro/

## git cherry-pick

Apply the changes introduced by some existing commits

### DESCRIPTION

Given one or more existing commits, apply the change each one introduces, recording a new commit for each. This requires your working tree to be clean (no modifications from the HEAD commit).

When it is not obvious how to apply a change, the following happens:

1. The current branch and `HEAD` pointer stay at the last commit successfully made.
2. The `CHERRY_PICK_HEAD` ref is set to point at the commit that introduced the change that is difficult to apply.
3. Paths in which the change applied cleanly are updated both in the index file and in your working tree.
4. For conflicting paths, the index file records up to three versions, as described in the "TRUE MERGE" section of [git-merge[1\]](https://git-scm.com/docs/git-merge). The working tree files will include a description of the conflict bracketed by the usual conflict markers *<<<<<<<* and *>>>>>>>*.
5. No other modifications are made.

### 指定commits的方式

1. commit hash

   直接使用每个提交的40个字符的SHA-1 哈希。当足够表示某个提交时，也可以只使用哈希的前面若干个字符

2. 当需要指定的commit 处于某个分支的顶端的时候，使用分支名就可以

3. 使用 ^ 或者 ~

   1. {commit}^ 表示 commit 的父提交，^ 后面可以加数字n表示提交的第n个父亲，加数字的情况一般用于合并提交。

   2. {commit}~ 也表示commit的父提交，~后面也可以加数字。

      HEAD~3 表示 HEAD的第一个父提交的第一个父提交的第一个父提交。 HEAD~3 == HEAD~~~

4. 指定一个 commit 范围

   可以使用 双点符号`..` 指定一个范围的提交。 比如 master..experiment 表示 experiment可达但是master 不可达的提交。

   如果提交历史如下：

   ```c
             A---B---C experiment
            /
       D---E---F---G master
   ```

    那么 `master..experiment` 表示的提交范围是 `A-B-C` 三个提交。

### 内部工作原理

工作原理类同 `git rebase` ,也是先计算`git cherry-pick` 参数指定的commits 引入的修改diff,临时保存下来，然后逐个应用到当前分支，并且每个diff 生成一个提交。如何有冲突则处理冲突。

Official Ref: https://git-scm.com/docs/git-cherry-pick

