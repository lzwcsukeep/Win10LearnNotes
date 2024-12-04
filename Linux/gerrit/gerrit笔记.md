## gerrit

### 概念

一个commit 就是一个change,In Gerrit, a single commit is the unit of code that will be reviewed, 一个change 可以有多个patch set，通过`git commit --amend` 产生一个patch set。这些patch set 同属于一个change(也是commit)

如果直接使用 `git commit` 命令提交，会生成第二个change。




