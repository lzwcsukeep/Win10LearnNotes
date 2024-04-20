dup2(int oldfd,int newfd) ;

把newfd 的输入输出重定向到oldfd。



dup(int oldfd)

功能同dup2,使用最低的没有被使用的fd作为newfd,返回newfd.
