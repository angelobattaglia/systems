# I/O Primitives in C, part I

## Accepting CLI arguments:

```c
int main(int argc, char ** argv){
    // in the code, to call them: argv[1], argv[2] ...
    // argv[0] is the name of the executable itself, say, "a.out"
    // so it goes: ./a.out argument1 argument2 ..
}
```

## Char vectors

1 char = 1 byte

```c
char name[1024];
```

Strings in C end in a NULL element. So the maximum
number that can be contained in a [1024] vector of
char is 1023 char, plus the NULL element "\0".

[1024] is the contiguos number of char allocated on the stack.

## Buffer(s)

A buffer is a temporary storage space used to read files,
to perform I/O operations or to manipulate data.

## Overview on the I/O primitives

These are standard ANSI C I/O functions for file operations

### **Comparison of Their Uses**
| Function     | Purpose                         | Level of Operation       |
|--------------|---------------------------------|--------------------------|
| **fgetc**    | Read a single character         | Character-based          |
| **fputc**    | Write a single character        | Character-based          |
| **fscanf**   | Read formatted data             | Formatted input          |
| **fprintf**  | Write formatted data            | Formatted output         |
| **fgets**    | Read a string/line              | Line or string-based     |
| **fputs**    | Write a string/line             | Line or string-based     |

---

### **When to Use Which**
- **`fgetc` and `fputc`**:
  - Use when you need to process files character by character, such as counting
   characters or detecting specific ones.
  
- **`fscanf` and `fprintf`**:
  - Use when working with structured data where formatting matters (e.g., reading
   integers, floats, or a mix of types from a file).

- **`fgets` and `fputs`**:
  - Use when working with text files line-by-line or dealing with strings.


## `sprintf`

The function `sprintf` writes formatted (by you) data to a string, instead of
standard output, like `printf` does.

```c
char name[1024];

sprintf(name, "%s.1", argv[2]);
```

say argv[2] is `test`, then it writes onto name the string: `test.1`

## Reading a file using `fscanf`

```c
FILE *destination, *source;
char c;

while(fscanf(source, "%c", &c) != EOF){
    fprintf(destination, "%c", c);
}
```

