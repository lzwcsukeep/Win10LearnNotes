#### Automatic Variables in make

*Automatic variables* are set by `make` after a rule is matched. They provide access to elements from the target and prerequisite lists so you don’t have to explicitly specify any filenames. They are very useful for avoiding code duplication, but are critical when defining more general pattern rules.

There are seven “core” automatic variables:

- `$@`: The filename representing the target.

- `$%`: The filename element of an archive member specification.

- `$<`: The filename of the first prerequisite.

- `$?`: The names of all prerequisites that are newer than the target, separated by spaces.

- `$^`: The filenames of all the prerequisites, separated by spaces. This list has duplicate filenames removed since for most uses, such as compiling, copying, etc., duplicates are not wanted.

- `$+`: Similar to `$^`, this is the names of all the prerequisites separated by spaces, except that `$+` includes duplicates. This variable was created for specific situations such as arguments to linkers where duplicate values have meaning.

- `$*`: The stem of the target filename. A stem is typically a filename without its suffix. Its use outside of pattern rules is discouraged.

For example:

```makefile
hello.o: hello.c hello.h
         gcc -c $< -o $@
```

Here, `hello.o` is the output file. This is what `$@` expands to. The first dependency is `hello.c`. That's what `$<` expands to.
