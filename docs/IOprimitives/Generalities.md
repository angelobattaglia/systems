# I/O made with `<stdio.h>`

- [Casting in C](https://www.skenz.it/cs/c_language/casting)
- [Functions in C](https://www.skenz.it/cs/c_language/functions_1)
- [Structs and typedef in C](https://www.skenz.it/cs/c_language/struct_and_typedef_1)
- [File Reading](https://www.skenz.it/cs/c_language/file_reading_1)
- [File Writing](https://www.skenz.it/cs/c_language/file_writing_1)
- [Command line arguments](https://www.skenz.it/cs/c_language/argc_and_argv)
- [Writing and Reading a binary file](https://www.skenz.it/cs/c_language/write_and_read_a_binary_file)



## Syscalls to manage files in Linux




## Arguments passed on the command line

```c
int main(int argc, char *argv[]);
```

* `argv[0]` is the string your operating system uses to invoke the program (often the path or name of the executable).
* `argv[1]` is the **first** actual command-line argument you passed.
* `argv[2]` would be the second argument, and so on, up to `argv[argc-1]`.

So if you run

```bash
./a.out 4
```

then inside `main`:

* `argc == 2`,
* `argv[0]` points to `"./a.out"`,
* `argv[1]` points to `"4"`.

Be careful: if you access `argv[1]` without checking that `argc > 1`, you’ll invoke undefined behavior when no argument was provided.

