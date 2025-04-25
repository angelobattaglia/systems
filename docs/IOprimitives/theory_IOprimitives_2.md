# I/O Primitives in C, part II

## fgetc/fputc

The following is one whole program that reads char-by-char from a file and write said content onto another one
```c
#include <stdio.h>
int main (int argc, char **argv) { // This line allows me to execute this program on the prompt by accepting CL arguments
  FILE *fs, *fd;
  char c;
  char name[1024];

  // Solution using fgetc and fputc
  sprintf (name, "%s.2", argv[2]);
  fs = fopen(argv[1], "r");
  fd = fopen(name, "w");
  while ((c = fgetc(fs)) != EOF) {
    fputc (c, fd);
  }
  fclose(fs);
  fclose(fd);

  return 0;
}
```

This is the portion worth analizing
```c
  fs = fopen(argv[1], "r");
  fd = fopen(name, "w");
  while ((c = fgetc(fs)) != EOF) {
    fputc (c, fd);
  }
```

- `c = fgetc(fs)`: reads the "next" char from the file "fs" and stores it into the `char c` variable
- `(c = fgetc(fs)) != EOF`: the expression `(c = fgetc(fs))` is equivalent to a single char type variable, when it is not a EOF variable
- `fputc(c, fd);`: writes the `char c` onto fd


## fscanf/fprintf

## fgets/fputs

