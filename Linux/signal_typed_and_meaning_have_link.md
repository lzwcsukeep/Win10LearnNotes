The SIGABRT is **explicitly detected and signaled** by the implementation of *delete* whose detected the invalidity of the second delete. It is launch by calling the *abort* function

The SIGSEGV is different, it is **undergoing** rather than detected by a check in a library like the previous, it is launch through the memory management of the OS

Here's a nice listing of what the various signals mean

[SIGTERM, SIGSEGV, SIGINT, SIGILL, SIGABRT, SIGFPE - cppreference.com](https://en.cppreference.com/w/c/program/SIG_types)
