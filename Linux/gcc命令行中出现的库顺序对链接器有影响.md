直接原链接里面读：

https://eli.thegreenplace.net/2013/07/09/library-order-in-static-linking

这里摘抄一些重点。

### The basics

Let's start by defining the scope of this article: first, my examples are demonstrating the use of the gcc and binutils toolchain on Linux. Compatible toolchains (like clang instead of gcc) apply too. Second, the discussion here resolves around *static* linking that's done at compile/link time.

To understand why linking order matters, it's first instructional to understand how the linker works with respect to linking libraries and objects together. Just as a quick reminder - an object file both *provides* (exports) external symbols *to* other objects and libraries, and *expects* (imports) symbols *from* other objects and libraries. For example, in this C code:

```c
int imported(int);

static int internal(int x) {
    return x * 2;
}

int exported(int x) {
    return imported(x) * internal(x);
}
```

The names of the functions speak for themselves. Let's compile it and look at the symbol table:

```bash
$ gcc -c x.c
$ nm x.o
000000000000000e T exported
                 U imported
0000000000000000 t internal
```

This means: exported is an external symbol - defined in the object file and visible from the outside. imported is an undefined symbol; in other words, the linker is expected to find it elsewhere. When we talk about linking later, the term *undefined* can become confusing - so it helps to remember that this is where it comes from originally. internal is defined within the object but invisible from the outside.

Now, a *library* is simply a collection of object files. Just a bunch of object files glued together. Creating a library is a very trivial operation that doesn't do anything special besides placing many object files into the same file. This in itself is important, because a horde of object files is not convenient to deal with. For example, on my system libc.a (the static version of the C library) consists of almost 1500 object files. It's way nicer to just carry libc.a around.

### The linking process

This section defines the linking process in a somewhat dry, algorithmic manner. This process is the key to understanding why linking order matters.

Consider a linker invocation:

```bash
$ gcc main.o -L/some/lib/dir -lfoo -lbar -lbaz
```

The linker is almost always invoked through the compiler driver gcc when compiling C or C++ code. This is because the driver knows how to provide the correct command-line arguments to the linker itself (ld) with all the support libraries, etc. We'll see more of this later.

Anyhow, as you can see the object files and libraries are provided in a certain order on the command-line, from left to right. This is the linking order. Here's what the linker does:

- The linker maintains a *symbol table*. This symbol table does a bunch of things, but among them is keeping two lists:
  - A list of symbols exported by all the objects and libraries encountered so far.
  - A list of undefined symbols that the encountered objects and libraries requested to import and were not found yet.
- When the linker encounters a new object file, it looks at:
  - The symbols it exports: these are added to the list of exported symbols mentioned above. If any symbol is in the undefined list, it's removed from there because it has now been found. If any symbol has already been in the exported list, we get a "multiple definition" error: two different objects export the same symbol and the linker is confused.
  - The symbols it imports: these are added to the list of undefined symbols, unless they can be found in the list of exported symbols.
- When the linker encounters a new library, things are a bit more interesting. The linker goes over all the objects in the library. For each one, it first looks at the symbols it exports.
  - If any of the symbols it exports are on the undefined list, the object is added to the link and the next step is executed. Otherwise, the next step is skipped.
  - If the object has been added to the link, it's treated as described above - its undefined and exported symbols get added to the symbol table.
  - Finally, if *any* of the objects in the library has been included in the link, the library is rescanned again - it's possible that symbols imported by the included object can be found in other objects within the same library.

When the linker finishes, it looks at the symbol table. If any symbols remain in the undefined list, the linker will throw an "undefined reference" error. For example, when you create an executable and forget to include the file with the main function, you'll get something like:

```plaintext
/usr/lib/x86_64-linux-gnu/crt1.o: In function '_start':
(.text+0x20): undefined reference to 'main'
collect2: ld returned 1 exit status
```

Note that after the linker has looked at a library, it won't look at it again. Even if it exports symbols that may be needed by some later library. The only time where a linker goes back to rescan objects it has already seen happens within a single library - as mentioned above, once an object from some library is taken into the link, all other objects in the same library will be rescanned. Flags passed to the linker can tweak this process - again, we'll see some examples later.

Also note that when a library is examined, an object file within it can be left out of the link if it does not provide symbols that the symbol table needs. This is a very important feature of static linking. The C library I mentioned before makes a heavy use of this feature, by mostly splitting itself to an-object-per-function. So, for example if the only C standard library function your code uses is strlen, only strlen.o will be taken into the link from libc.a - and your executable will be very small.

个人理解：

1. 链接器会维护两个表exported表，undefined 表，这两个表对所有的object来说的全局的。

2. 每一个object file 里的符号分两种export, import. export 表示自己定义的，对外部可见的符号，import 表示需要从外部引入的。

3. 链接器每遇到一个object 文件，查看当前object 里的export 符号和 import 符号，据此更新全局的exported和undefined 表。

4. 静态库的本质就是多个object file 的集合而已，除此之外没有特殊之处。当遇到库的时候，链接器的处理类似，但是稍微有点区别。细看文章中的区别之处。

5. 链接器的处理是从左到右处理目标文件进行链接的。库文件出现的顺序会影响链接结果。

也就是 linking order matters！

> To understand why **linking order matters**, it's first instructional to understand how the linker works with respect to linking libraries and objects together. Just as a quick reminder - an object file both *provides* (exports) external symbols *to* other objects and libraries, and *expects* (imports) symbols *from* other objects and libraries.




