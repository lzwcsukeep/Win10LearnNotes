generally speaking ,git diff compares the differences between files.
but in git ,there are different area containing files,i.e.`working directory`
,`staged` and `commits`.so git diff may compare files from different area when using it. 
To be short

To see what you’ve changed but not yet staged, type git diff with no other arguments:

git diff without option compares what is in your working directory with what is in your
staging area.To see what you have changed but not yet staged.

but git diff with option `--staged` compares your staged changes to your last commit.
this help you to see what you have staged that will go into your next commmit.

so, in a word ,git diff can compares files from different area,its action is not always the
same(between no option and with option --staged).

一个是比较当前工作区和staged area来看看你有哪个没有提交到暂存区，
一个是比较暂存区和上次提交来看看这次准备提交的东西有什么区别。
untracked file 不属于git的working tree!!!!!





---------------------------

To see what you’ve changed but not yet staged, type `git diff` with no other arguments:

```c
$ git diff
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branch directly, things can get messy.
 Please include a nice description of your changes when you submit your PR;
 if we have to read the whole diff to figure out why you're contributing
 in the first place, you're less likely to get feedback and have your change
-merged in.
+merged in. Also, split your changes into comprehensive chunks if your patch is
+longer than a dozen lines.
 If you are starting to work on a particular area, feel free to submit a PR
 that highlights your work in progress (and note in the PR title that it's
```

That command compares what is in your working directory with what is in your staging area. The result tells you the changes you’ve made that you haven’t yet staged.

If you want to see what you’ve staged that will go into your next commit, you can use `git diff --staged`. This command compares your staged changes to your last commit:

```c
$ git diff --staged
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```

It’s important to note that git diff by itself doesn’t show all changes made since your last commit — only changes that are still unstaged. If you’ve staged all of your changes, git diff will give you no output.
