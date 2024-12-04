两个时间点进行相减，得到一个duration(时间段)。

即两个`time_point` 对象相减，得到一个`duration` 对象。

### 关于C++中的时间会涉及的概念：

- duration

时间段，表示一个时间段，如一分钟，两个小时，10毫秒。

在`<chrono>` 中是模板类`duration`的对象，有两个要素，一个是计数值，另一个是时间单位。 比如10毫秒的计数值是10，时间单位是毫秒。

`chrono`中有表示时分秒,微秒，纳秒的`std::chrono::hours`,`minutes`,`seconds`,`milliseconds`,`microseconds`,`nanoseconds`(省略了std::chrono)，这些都是模板类`duration`的实例类(Instantiation)

如: `std::chrono::seconds sec(8) ;` 定义了一个时间段为8秒的`duration` 对象sec。

- Time points

表示一个具体的时间点，比如一个人的生日，黄昏时刻，火车到达点。

在`chrono`中模板类`time_point`对象通过使用相对创世纪时刻的时间段`duration`来表达时间点。也就是说`time_point`还是通过使用上面的时间段类`duration`来表达时间点的，不过起始时间点是创世纪时刻。

- Clocks 时钟

A framework that relates a *time point* to real physical time.  
The library provides at least three clocks that provide means to express the current time as a [time_point](https://cplusplus.com/time_point): `system_clock`, `steady_clock` and `high_resolution_clock`

获取当前时间：

上述三个时钟都有一个静态成员方法`now()` 用来获取当前时间。
