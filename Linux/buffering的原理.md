### IO buffer 的笔记：

buffer是在内存里面，read,write都是对磁盘操作。

有了buffer之后，尽管只读一个字节，但是可以先从磁盘读一大片放在buffer里面。

尽管只写一个字节，可以先写到buffer里面，达到一定数量，再一次性写入。上面两个buffer不是同一个buffer?

- buffer 发生在内存与磁盘之间进行数据交换时。

- 如果没有buffer,那么每次执行一个read,write(对象都是磁盘)都会执行一次系统调用。

- 有了buffer 之后，每次read，write 时都不立刻进行磁盘操作，而是先把数据放到buffer 里面，等达到一定条件时在一次性写入大量数据。

- buffer 在内存里面，而不是磁盘上。

- 根据一次性写入的条件,buffering 可以有几种模型：

**Types of Buffering**:

The standard I/O library provides three types of buffering:

1. **Fully Buffered**: Data is read or written in large chunks (e.g., 4 KB or 8 KB) until the buffer is filled or flushed.

2. **Line Buffered**: Data is read or written line by line, meaning that it waits until a newline character (`\n`) is encountered before performing the I/O operation.

3. **Unbuffered**: Data is read or written directly without buffering. Each read or write operation directly corresponds to a system call.
- 当使用`fread`, `fwrite`, `fprintf`, or `fscanf` 时, the library manages the buffering for you behind the scenes.The application doesn't need to explicitly manage or implement buffering; the library takes care of it.

- 可以使用`fflush`函数，手动刷新缓存而不等待条件满足时才清空缓存。

下面是详细的Buffer 工作流程：

### how buffering works?

Buffering works by temporarily storing data in a memory buffer before it is read from or written to a file or another I/O device. This process helps optimize I/O operations by reducing the frequency of actual read and write system calls, which tend to be slower compared to accessing data in memory.

Here's how buffering typically works:

1. **Data Accumulation**: When data is written to an output stream (e.g., a file), instead of immediately writing it to the underlying device, the data is accumulated in a buffer in memory.

2. **Buffer Management**: The standard I/O library manages these buffers. It keeps track of the data stored in each buffer and handles operations like flushing buffers to write their contents to disk.

3. **Buffer Size**: Buffers have a fixed size, which can vary depending on the system and the specific I/O operation. The size of the buffer affects the trade-off between memory usage and the frequency of system calls.

4. **Flush Operation**: Buffers are flushed when certain conditions are met:
   
   - When the buffer is full: If the buffer reaches its maximum capacity, its contents are flushed to the underlying device.
   - When explicitly requested: The programmer can explicitly request the flushing of a buffer using functions like `fflush` in C.
   - When a newline character is encountered (for line-buffered streams): In line-buffered mode, the buffer is flushed whenever a newline character (`\n`) is encountered in the data being written.

5. **Read Operations**: When reading from an input stream, data is read into the buffer from the underlying device in larger chunks. Subsequent read requests are served from the buffer until it's empty, at which point more data is fetched from the device.

Benefits of buffering include:

- Reducing the number of system calls, which can be expensive in terms of performance.
- Smoothing out I/O operations by grouping them into larger, more efficient chunks.
- Minimizing contention for shared resources like disk I/O, which can improve overall system performance.

However, buffering also introduces some considerations, such as the possibility of data loss if the system crashes before buffered data is written to disk, or the need to explicitly flush buffers in certain cases to ensure data integrity. Understanding how buffering works is crucial for writing efficient and reliable I/O code.

Links:

https://chat.openai.com/share/fafbea10-a48d-4862-8348-5c6180ae98f0
