## C++ 中condition_variable的笔记

wait() 函数的使用：
1. wait() 函数有两种形式，一种是void wait (unique_lock<mutex>& lck);
另一种是template <class Predicate>  void wait (unique_lock<mutex>& lck, Predicate pred);
第一种形式是无条件阻塞，也就是说，当前线程只要调用了第一种wait(unique_lock<mutex>& lck)方法，就会阻塞当前线程，并且wait(unique_lock<mutex>& lck)函数还会调用unlock()函数，释放阻塞线程拥有的锁，以方便另外的线程获得锁而执行。
第二种形式是有条件阻塞void wait (unique_lock<mutex>& lck, Predicate pred);该wait()方法可以传入一个条件，当条件为真时不会阻塞。当条件为假时才阻塞。这个函数可以用来让线程等待某个条件成熟在执行。也就是说，当某个条件为真时，该线程即使调用了void wait (unique_lock<mutex>& lck, Predicate pred);该线程也不会阻塞。

无论使用哪种wait()函数，只要线程阻塞了就会释放锁，被唤醒就会重新获得锁。


notify() 函数功能：
