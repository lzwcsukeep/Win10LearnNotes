C++成员函数声明后面使用const，表明这个函数内部的操作不应该修改该类的数据成员。
如果修改数据成员，编译器会报错。
好处是给开发人员一个提醒。快速的知道这个函数不应该修改类的成员数据。

> A "const function", denoted with the keyword const after a function declaration, makes it a compiler error for this class function to change a data member of the class. However, reading of a class variables is okay inside of the function, but writing inside of this function will generate a compiler error.
