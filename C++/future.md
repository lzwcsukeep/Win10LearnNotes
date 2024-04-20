# Future

A future is an object that can retrieve a value from some provider object or function, properly synchronizing this access if in different threads.  

"Valid" futures are [future](https://cplusplus.com/future) objects associated to a *shared state*, and are constructed by calling one of the following functions:  

- [async](https://cplusplus.com/async)
- [promise::get_future](https://cplusplus.com/promise::get_future)
- [packaged_task::get_future](https://cplusplus.com/packaged_task::get_future)

[future](https://cplusplus.com/future) objects are only useful when they are *[valid](https://cplusplus.com/future::valid)*. *[Default-constructed](https://cplusplus.com/future::future)* [future](https://cplusplus.com/future) objects are not *[valid](https://cplusplus.com/future::valid)* (unless [move-assigned](https://cplusplus.com/future::operator=) a *[valid](https://cplusplus.com/future::valid)* [future](https://cplusplus.com/future)).  

Calling [future::get](https://cplusplus.com/future::get) on a *[valid](https://cplusplus.com/future::valid)* [future](https://cplusplus.com/future) blocks the thread until the *provider* makes the *shared state* ready (either by setting a value or an exception to it). This way, two threads can be synchronized by one waiting for the other to set a value.  

The lifetime of the *shared state* lasts at least until the last object with which it is associated releases it or is destroyed. Therefore, if associated to a [future](https://cplusplus.com/future), the *shared state* can survive the object from which it was obtained in the first place (if any).
