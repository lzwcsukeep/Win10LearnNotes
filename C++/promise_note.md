# The notes about using promise in C++

promise is one of provider in C++ asynchronous programming,it can be used 
with future together.

promise can use member function `get_future` to obtain the associated future object.

then the associated future object can call future::get() function to retrieve the result 

set by promise object by calling `set_value()` function.

promise 也是线程间异步通信的手段之一。
