## ar - create, modify, and extract from archives

The GNU `ar` program creates, modifies, and extracts from archives.  An `archive` is a single file holding a collection of other files in a structure that makes it
       possible to retrieve the original individual files (called members of the archive). 

`SYNOPSIS`

ar [-X32_64] [-]p[mod] [--plugin name] [--target bfdname] [--output dirname] [relpos] [count] archive [member...]

创建修改提取archives 的，至于archives 是什么，定义就在上面。

archives里的文件，叫做archives的member。

## nm - list symbols from object files

GNU `nm` lists the symbols from object files `objfile....` If no object files are listed as arguments, `nm` assumes the file `a.out`.

`SYNOPSIS`
       nm [-A|-o|--print-file-name] [-a|--debug-syms]
          [-B|--format=bsd] [-C|--demangle[=style]]
          [-D|--dynamic] [-fformat|--format=format]
          [-g|--extern-only] [-h|--help]
          [-l|--line-numbers] [--inlines]
          [-n|-v|--numeric-sort]
          [-P|--portability] [-p|--no-sort]
          [-r|--reverse-sort] [-S|--print-size]
          [-s|--print-armap] [-t radix|--radix=radix]
          [-u|--undefined-only] [-V|--version]
          [-X 32_64] [--defined-only] [--no-demangle]
          [--plugin name]
          [--no-recurse-limit|--recurse-limit]]
          [--size-sort] [--special-syms]
          [--synthetic] [--with-symbol-versions] [--target=bfdname]
          [objfile...]

For each symbol, `nm` shows:

- The symbol value, in the radix selected by options (see below), or hexadecimal by default.

- The symbol type.  At least the following types are used; others are, as well, depending on the object file format.  If lowercase, the symbol is usually local
  ;if uppercase, the symbol is global (external).  There are however a few lowercase symbols that are shown for special global symbols ("u", "v" and "w").

- The symbol name.

上面讲的是`nm`命令输出的内容格式。

即三部分：符号的值，符号的类型，符号的名字。示例如下：

```
lizhiwen@lizhiwen:~/workspace/learn$ nm libtt.a 

file_read.o:
                 U fclose
                 U fgets
                 U fopen
                 U fprintf
                 U _GLOBAL_OFFSET_TABLE_
0000000000000000 T main
                 U printf
                 U __stack_chk_fail
                 U stderr

test.o:
0000000000000000 T add
0000000000000018 T sub
```

符号类型字母含义：

"U" The symbol is undefined.

"T"
"t" The symbol is in the text (code) section.

complete symbol type please using `man nm`.

## ranlib - generate an index to an archive

`SYNOPSIS`
       ranlib [--plugin name] [-DhHvVt] archive

`DESCRIPTION`
       `ranlib` generates an index to the contents of an archive and stores it in the archive.  The index lists each symbol defined by a member of an archive that is a relocatable object file.

You may use `nm -s` or `nm --print-armap` to list this index.
An archive with such an index speeds up linking to the library and allows routines in the library to call each other without regard to their placement in the archive.
       

The GNU `ranlib` program is another form of GNU `ar`; running `ranlib` is completely equivalent to executing `ar -s`.

`ranlib`是给archive文件里的符号建立索引的，索引可以加快链接库的速度。

`ranlib`等价于`ar -s`




