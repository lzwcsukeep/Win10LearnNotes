### Native C/C++ libraries

Many core Android system components and services, such as ART and HAL, are built from native code that requires native libraries written in C and C++。

### ART

Android Runtime（ART）主要是用C++编写的。ART是安卓系统的应用程序运行时环境，从Android 5.0（Lollipop）开始，ART取代了之前的Dalvik虚拟机，成为默认的运行时。

### 具体细节

1. **核心实现语言**：
   
   - **C++**：ART的核心部分使用C++编写。C++提供了高效的低级别系统编程能力，这对于实现一个高性能的运行时环境至关重要。

2. **辅助组件和接口**：
   
   - **Java**：ART与安卓应用及系统服务交互的接口部分使用Java编写。应用程序运行在ART之上，通过Java API与系统进行交互。
   - **汇编语言**：某些性能关键的部分，例如即时编译器（JIT）和垃圾收集器（GC），可能使用汇编语言来实现，以确保最高效的执行性能。

3. **JNI（Java Native Interface）**：
   
   - ART使用JNI来调用本地C/C++代码。JNI是一种编程框架，它允许Java代码与用其他编程语言编写的本地代码进行交互，这在ART中用于实现某些底层功能

### ART的关键功能

- **AOT（Ahead-of-Time）编译**：ART在应用安装时对应用代码进行AOT编译，将字节码编译成本地机器代码，以提高应用的启动速度和运行时性能。
- **JIT（Just-in-Time）编译**：在应用运行时，ART还可以进行JIT编译，根据实际使用情况进一步优化应用代码。
- **垃圾收集（Garbage Collection）**：ART实现了高效的垃圾收集算法，以管理和回收应用程序不再使用的内存。
- **性能分析（Profiling）**：ART提供了性能分析工具，帮助开发者识别和优化性能瓶颈。

### 总结

Android Runtime（ART）的核心部分主要使用C++编写，辅以Java和汇编语言。ART通过AOT和JIT编译、垃圾收集和性能分析等技术，为安卓应用提供了高效的运行时环境。
