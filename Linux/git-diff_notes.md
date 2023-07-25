generally speaking ,git diff compares the differences between files.
but in git ,there are different area containing files,i.e.`working directory`
,`staged` and `commits`.so git diff may compare files from different area when using it. 
To be short

git diff without option compares what is in your working directory with what is in your
staging area.To see what you have changed but not yet staged.

but git diff with option `--staged` compares your staged changes to your last commit.
this help you to see what you have staged that will go into your next commmit.

so, in a word ,git diff can compares files from different area,its action is not always the
same(between no option and with option --staged).


一个是比较当前工作区和staged area来看看你有哪个没有提交到暂存区，
一个是比较暂存区和上次提交来看看这次准备提交的东西有什么区别。
untracked file 不属于git的working tree!!!!!
