## Compilers

#### References
- [Inlcude guard (wikipedia link)](https://en.wikipedia.org/wiki/Include_guard)
- [Systems Programming in UNIX Linux by K.C. Wang, Springer]()
- [Driving Compilers](https://fabiensanglard.net/dc/index.php)

#  Compiling two files into a binary

Simply compiling two files into one executable
```
gcc Script01.c Script02.c
```

## The `-c` flag

When you compile a file using `gcc` with the `-c` flag, the process stops after the compilation phase and generates an **object file** (`Script02.o`) instead of a final executable. Here's what this means:

### **Breaking it Down**

1. **Source Code (`Script02.c`)**: 
   - The `.c` file is a text file containing the C source code.

2. **Compilation with `-c`**: 
   - The `gcc -c` command translates the source code into **machine code**, creating an object file (`Script02.o`).
   - It doesn’t link the object file with other object files or libraries to create an executable at this stage.

3. **Object File (`Script02.o`)**: 
   - This file contains the compiled machine code but is not standalone or executable. 
   - It needs to be **linked** with other object files and libraries in a later step to produce an executable.

### **Why Use `-c`?**
- **Modularity**: In larger projects, code is split across multiple source files. Compiling each file individually into object files speeds up the process. If one file changes, you only need to recompile that file, not the entire project.
- **Error Isolation**: Errors can be found and fixed in each source file individually before moving to the linking phase.
- **Separation of Compilation and Linking**: It separates the code's transformation to machine instructions (compilation) from combining those instructions into a runnable program (linking).

### Example Usage
```bash
gcc -c Script02.c       # Compiles to Script02.o
gcc -o program Script02.o   # Links to create 'program' (the executable)
```

### In Short:
Using `gcc -c` means **"compile only"**—you are generating intermediate object files for later use, not a complete program.

## Exporting a library path

Exporting a library path typically means setting an environment variable that tells the operating system or a program where to find shared libraries (or dynamic link libraries, depending on your system) when they are needed at runtime. This is especially common in Unix-like systems such as Linux and macOS.

The **library path** refers to a directory or a set of directories where the system should look for shared libraries. By exporting this path, you make it available to processes that are spawned by the current shell session.

### Why export a library path?
- To allow programs to locate and use shared libraries that are not installed in the system's default library directories (e.g., `/lib`, `/usr/lib`).
- To override system-wide library paths with custom ones, such as libraries installed in user-specific locations.
- To facilitate testing or development with specific versions of libraries.

### How is it done?
On Linux and Unix-like systems, the `LD_LIBRARY_PATH` environment variable is commonly used. You export it in a shell like this:

```bash
export LD_LIBRARY_PATH=/path/to/your/library:$LD_LIBRARY_PATH
```

- `/path/to/your/library`: The directory containing the libraries you want to use.
- `$LD_LIBRARY_PATH`: Appends the new path to the existing library paths to preserve them.

### Example
Suppose you have a custom library in `/home/angelo/custom_lib`, and you want your program to use it. You would run:

```bash
export LD_LIBRARY_PATH=/home/angelo/custom_lib:$LD_LIBRARY_PATH
```

Now, when you run a program that requires this library, the system will search `/home/angelo/custom_lib` first.

### For Windows
On Windows, library paths are typically managed by setting the `PATH` environment variable to include the directory of your `.dll` files.

For example:

```cmd
set PATH=C:\path\to\your\library;%PATH%
```

This ensures the operating system knows where to look for `.dll` files at runtime.

## Concepts regarding the PATH variables for executables and libraries

---

### **1. Shell Configuration Files (`.bashrc`, `.profile`, etc.)**
- The **shell** (e.g., Bash, Zsh) is your interface to the operating system. Configuration files like `~/.bashrc` or `~/.profile` are used to customize the shell environment for your user.
- These files are **not directly involved in compilation or execution** but are used to set up environment variables (like `PATH` and `LD_LIBRARY_PATH`) that influence how the operating system and shell behave when you compile or run programs.

---

### **2. Environment Variables and the Role of `PATH` and `LD_LIBRARY_PATH`**
When you modify these variables in `~/.bashrc` or `~/.profile`, you’re instructing the shell to configure paths that the operating system uses:

#### **`PATH` for Executables**
- When you run a command (e.g., `gcc`, `python`), the shell searches the directories listed in the `PATH` environment variable to locate the executable.
- Example:
  ```bash
  export PATH=/usr/local/cuda/bin:$PATH
  ```
  - This tells the shell to include `/usr/local/cuda/bin` (where CUDA executables like `nvcc` are stored) in the list of directories it checks when you run a command.

#### **`LD_LIBRARY_PATH` for Libraries**
- When a program is executed, it might rely on shared libraries (like `.so` files). The operating system uses `LD_LIBRARY_PATH` to find these libraries at runtime.
- Example:
  ```bash
  export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
  ```
  - This tells the operating system to include `/usr/local/cuda/lib64` (where CUDA libraries like `libcudart.so` are stored) when searching for shared libraries.

---

### **3. Compilation and Execution in Unix Systems**
- **Compiling**: When you compile a program (e.g., with `gcc`), the compiler looks for:
  - Executables (e.g., `gcc`, `nvcc`): Found via the `PATH`.
  - Header files (`.h`): Searched in directories specified by `-I` or defaults like `/usr/include`.
  - Libraries (`.so`, `.a`): Searched in directories specified by `-L` or defaults like `/usr/lib`.
  
  If you set `PATH`, the shell can find compilers like `nvcc`. Similarly, if you pass flags like `-I` and `-L`, the compiler knows where to find headers and libraries.

- **Execution**: Once compiled, executables may rely on shared libraries. If the required libraries are not in standard paths, `LD_LIBRARY_PATH` must be set to help the operating system locate them.

---

### **4. The Role of `.bashrc` and `.profile`
- Files like `.bashrc` and `.profile` are only scripts that run when you start a shell session. They allow you to **automatically configure your environment**.
- For example, if you add the following to `~/.bashrc`:
  ```bash
  export PATH=/usr/local/cuda/bin:$PATH
  export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
  ```
  - Every new shell session will automatically have these variables set.
  - This ensures commands like `nvcc` work and the necessary CUDA libraries are found at runtime.

---

### **5. Summary of What Happens**
1. **At shell startup**: Files like `~/.bashrc` are sourced, setting environment variables.
2. **When you run a command**:
   - The shell uses `PATH` to locate the executable.
3. **When the program executes**:
   - The operating system uses `LD_LIBRARY_PATH` to locate required shared libraries.

The `.bashrc` or `.profile` files themselves are not magic; they simply automate the process of setting environment variables for the shell and, indirectly, the operating system.