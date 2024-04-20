- bit-field 的长度不能超过类型的长度

> 比如在struct 中声明一个 int m_a: 5 的 bit-field 成员m_a 。该m_a 的长度不能超过32位(假设该机器上int占用32位 )。

- 不光有字节序，还有位序（bit order），位序也分大小端，是一个字节内部各个bit的顺序。

> 比如声明一个struct t {   int a : 3 ;  }   如果 t.a = 3; 因为3的二进制是011，那么a 的内存表示可能是011(大端)或 110(小端。)

- bit-field 有存储单元(storage unit)的概念。存储单元的大小和bit-field 的类型有关。

- 声明中相邻的同类型的bit-field成员，如果可以在同一个存储单元放下的话则紧紧贴着对方放在同一个存储单元里。如果放不下，则是跨存储单元存放还是从新的存储单元开始放和平台以及编译器相关。

The C11 standard says:

> An implementation may allocate any addressable storage unit large enough to hold a bitfield. If enough space remains, a bit-field that immediately follows another bit-field in a structure shall be packed into adjacent bits of the same unit. If insufficient space remains, whether a bit-field that does not fit is put into the next unit or overlaps adjacent units is implementation-defined. The order of allocation of bit-fields within a unit (high-order to low-order or low-order to high-order) is implementation-defined.

我们还可以通过**匿名0长度位域字段**的语法强制位域在下一个存储单元开始分配：

如：

// 未使用 匿名0长度位域字段 

```c
struct short_flag_t {
    unsigned short a : 2,
                   b : 3;
};
// sizeof(short_flag_t ) = 2;
```

```c
struct short_flag_t {
    unsigned short a : 2;
// 虽然b可以和a放得下同一个存储单元，但是有了这句，b强制在下一个存储单元存放。
// 进而会增加struct的大小
    unsigned short   : 0;  
    unsigned short b : 3;
}; 

// sizeof(short_flag_t ) = 4 ;
```

有用的资源：

1. https://stackoverflow.com/questions/15136426/memory-layout-of-struct-having-bitfields

2. https://tonybai.com/2013/05/21/talk-about-bitfield-in-c-again/
