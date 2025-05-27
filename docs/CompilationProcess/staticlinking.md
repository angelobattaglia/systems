# Static Linking

- First step: compile Script02.c into Script02.o, important using the '-c' flag
```shell
gcc -c Script02.c
```

```shell
gcc -c Script02.c       # Compiles to Script02.o
gcc -o program Script02.o   # Links to create 'program' (the executable)
```

- Second step: create a Statically Linked Library with Script02.o being a part of
```
ar rcs libmylib.a Script02.o
```

- Third step: Static compile-link Script01.c with libmylib.a as a linked library
- '-static' specifies that is being statically linked
- '-L.' specifies the library path (current directory .)
- '-l' specifies the library
- 'mylib' is specified without the prefex lib, as well as the suffix .a
```
gcc -static Script01.c -L. -lmylib
```

- Fourth step: run the result, i.e. a.out, as usual
```
./a.out
```

# Static Linking

In the context of Linux and C/C++ programming, static and dynamic linking are two different methods of combining various pieces of code into a single executable program.
Linking a C++ script with a library during the compilation process through the Command Line Interface (CLI)
may vary depending on your operating system and development environment. In static linking, all the required code from external libraries and the application
code are combined into a single large executable file during the compile time. 
This includes copying the object code of used library functions directly into the final executable. When you compile your C/C++ program, the compiler links the object
code of your program with the object code of any libraries you used. These libraries are typically `.a` (archive) files on Linux. 
The end result is a standalone executable that contains all the necessary code, including the library functions.
The advantages are that the final executable does not depend on external libraries at runtime and that you're ensured that the library version your program was compiled
with is the one it runs with. It's also easier for the distribution as the executable is self-contained.
The disadvantages concern the size, since the executables can be larger since they include all library code, and updating a library which results in recompiling all dependent
programs. Also the resource usage, as multiple programs using the same library each have their own copy in memory, which can be optimized by just using one copy for them all.

## Steps:

1. **Write Your C++ Code:**
   Create your C++ source code file, for example, `main.cpp`.

   ```cpp
   // main.cpp
   #include <iostream>
   #include "mylibrary.h"

   int main() {
       // Your code here
       return 0;
   }
   ```

2. **Write the Library Code:**
   Create the library source code file, for example, `mylibrary.cpp`.

   ```cpp
   // mylibrary.cpp
   #include <iostream>

   void myLibraryFunction() {
       // Your library code here
   }
   ```

   Create the corresponding header file, for example, `mylibrary.h`.

   ```cpp
   // mylibrary.h
   #ifndef MYLIBRARY_H
   #define MYLIBRARY_H

   void myLibraryFunction();

   #endif // MYLIBRARY_H
   ```

3. **Compile Library:**
   Compile the library source code into a shared or static library. The exact command may vary based on your compiler and platform.

   ```bash
   g++ -c mylibrary.cpp -o mylibrary.o
   g++ -shared -o libmylibrary.so mylibrary.o  # For shared library (.so) on Linux
   ```

   Or for a static library:

   ```bash
   ar rcs libmylibrary.a mylibrary.o  # For static library (.a)
   ```

4. **Compile Main Code:**
   Compile your main C++ code, linking it with the library. You'll need to specify the library file and its path.

   ```bash
   g++ -o myprogram main.cpp -L/path/to/library -lmylibrary
   ```

   Replace `/path/to/library` with the actual path where your library file (`libmylibrary.so` or `libmylibrary.a`) is located.

5. **Run the Executable:**
   Run your compiled executable.

   ```bash
   ./myprogram
   ```

### Notes:

- The specific commands and flags may vary depending on your compiler (e.g., `g++` for GCC, `clang++` for Clang) and operating system.
- Ensure that the library file and its path are correctly specified during the compilation process.
- Adjust library names and file extensions based on your platform (e.g., `.so` for shared libraries on Linux, `.dylib` for macOS, `.dll` for Windows).
- If using header files (`mylibrary.h`), make sure they are in the correct location or include the appropriate `-I` flag to specify the include path.

Adapt the commands based on your specific requirements and development environment.

## Creating and packaging a statically linked library

Creating and packaging a static library on Linux using GCC involves several steps. I'll guide you through the process with a simple example.

Let's assume you have a library with two source files: `mylibrary.cpp` and `mylibrary.h`. We'll create a static library named `libmylibrary.a`.

### 1. Write Library Code:

Create `mylibrary.h`:

```cpp
// mylibrary.h
#ifndef MYLIBRARY_H
#define MYLIBRARY_H

void myLibraryFunction();

#endif // MYLIBRARY_H
```

Create `mylibrary.cpp`:

```cpp
// mylibrary.cpp
#include "mylibrary.h"
#include <iostream>

void myLibraryFunction() {
    std::cout << "Hello from myLibraryFunction!" << std::endl;
}
```

### 2. Compile Library:

Compile the library source code into an object file and then create the static library.

```bash
# Compile the source file into an object file
g++ -c mylibrary.cpp -o mylibrary.o

# Create the static library
ar rcs libmylibrary.a mylibrary.o
```

This will generate a static library file named `libmylibrary.a`.

### 3. Write Main Code:

Create a main program, e.g., `main.cpp`:

```cpp
// main.cpp
#include "mylibrary.h"

int main() {
    myLibraryFunction();
    return 0;
}
```

### 4. Compile Main Code with the Library:

Compile the main program, linking it with the static library.

```bash
g++ -o myprogram main.cpp -L. -lmylibrary
```

Here, `-L.` specifies the library search path (current directory), and `-lmylibrary` links against the `libmylibrary.a` static library.

### 5. Run the Executable:

Run your compiled executable:

```bash
./myprogram
```

This should print "Hello from myLibraryFunction!" to the console.

### Notes:

- Ensure that the library file (`libmylibrary.a`) is in the same directory or provide the correct path during the compilation.
- Adjust the compilation commands based on your specific project structure and requirements.
- The library file extension for static libraries is typically `.a` on Linux.
- You can distribute the `libmylibrary.a` file along with the header file (`mylibrary.h`) for others to use in their programs. They would then link against your static library when compiling their programs.

### Conclusion
- **Static Linking** is used when you want to ensure your program is self-contained and consistent, but at the cost of larger executable size and potentially more memory usage.
