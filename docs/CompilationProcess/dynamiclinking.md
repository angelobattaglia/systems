# Dynamic Linking

- First step: compile Script02.c into Script02.o, -fPIC compile to Position Independent Code Script02.o
```
gcc -c -fPIC Script02.c
```

- Second step: create a shared (-shared) Dinamically Linked Library (.so) with Script02.o as a member of it
```
gcc -shared -o libmylib.so Script02.o
```

- Third step: Static compile-link Script01.c with libmylib.a as a linked library
- '-L.' specifies the library path (current directory . or you can specify it)
- '-l' specifies the library as seen next
- '-lmylib' is specified without the prefex lib, as well as the suffix .a
```
gcc Script01.c -L. -lmylib
```

After installation, you need to add the CUDA binaries to your system's PATH and library paths. You can do this by adding the following lines to your `~/.bashrc` or `~/.profile` file:

```bash
# Add CUDA to PATH
export PATH=/usr/local/cuda/bin:$PATH
# Add CUDA libraries to LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

After adding these lines, reload your shell configuration:

```bash
source ~/.bashrc
```

- Third-2 step: exporting the LD_LIBRARY = ./
- If the library is not in the curent directory set LD_LIBRARY_PATH to point to the directory containing the library
```
export LD_LIBRARY_PATH = ./
```

- Fourth step: run the result, i.e. a.out, as usual
```
./a.out
```

# Dynamic Linking
- **Dynamic Linking** is more efficient in terms of disk space and memory, and it offers flexibility with library updates, but it introduces dependencies on external libraries and potential compatibility issues. 
This is a high-level overview, and there are more nuances, especially when you get into topics like dynamically loaded libraries (using `dlopen` and `dlsym` in C/C++), which allow for even more dynamic behavior.

Dynamic linking involves keeping the code of libraries separate from the executable. 
The linking happens at runtime, where the executable file contains references to the library functions it uses.

2. **How it Works**:
   - Dynamic libraries in Linux are typically `.so` (shared object) files.
   - When you compile your program, the compiler leaves placeholders for the addresses of the functions from the library.
   - At runtime, the Linux loader (`ld.so`) fills in these placeholders with actual addresses of the library functions, which are loaded into memory.

3. **Advantages**:
   - **Reduced Size**: Executables are smaller as they don't contain the actual library code.
   - **Shared Libraries**: Multiple programs can share the same library code in memory, reducing overall memory usage.
   - **Easy Updates**: Updating a library does not require recompiling the programs using it.

4. **Disadvantages**:
   - **Dependency**: Executables depend on external libraries being present on the system.
   - **Compatibility**: Issues can arise if the wrong version of a library is used at runtime.
   - **Startup Time**: Slightly longer startup times for programs as linking is done at runtime.


### 2. Library Dependencies

When you compile a binary, it often relies on shared libraries (dynamic libraries) that need to be present on the system for it to run. If these libraries are not found in the expected locations, the binary will fail to execute.

- **Checking Dependencies with `ldd`**:
  The `ldd` command prints the shared libraries required by a binary. For example:
  ```sh
  ldd /usr/bin/your_binary
  ```
  This command will output something like:
  ```
  linux-vdso.so.1 (0x00007ffd5ed9e000)
  libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007fbd5e144000)
  libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fbd5dd7b000)
  /lib64/ld-linux-x86-64.so.2 (0x00007fbd5e443000)
  ```
  If any libraries are marked as "not found," you need to install those libraries or make sure they are in the correct path.

- **Setting the Library Path**:
  If the libraries are installed in non-standard locations, you can set the `LD_LIBRARY_PATH` environment variable to include the paths where the libraries are located:
  ```sh
  export LD_LIBRARY_PATH=/path/to/custom/libs:$LD_LIBRARY_PATH
  ```
  You can add this line to your `.bashrc` or `.profile` to make it persistent.

- **Using `ldconfig`**:
  You can add the library path to `/etc/ld.so.conf` and run `ldconfig` to update the linker cache:
  ```sh
  echo "/path/to/custom/libs" | sudo tee -a /etc/ld.so.conf
  sudo ldconfig
  ```

### 5. Environment Variables

Environment variables can significantly affect how a binary runs. These variables provide configuration and setup information that the binary might depend on.

- **Common Environment Variables**:
  Some binaries might rely on variables like `PATH`, `LD_LIBRARY_PATH`, `PYTHONPATH`, `JAVA_HOME`, etc. Make sure these are set correctly:
  ```sh
  export PATH=/usr/bin:$PATH
  export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH
  ```

- **Checking and Setting Variables**:
  You can check the current value of an environment variable using `echo`:
  ```sh
  echo $VARIABLE_NAME
  ```
  To set an environment variable for the current session:
  ```sh
  export VARIABLE_NAME=value
  ```

- **Persistent Settings**:
  To make these changes persistent, add them to your shell’s configuration file (e.g., `.bashrc`, `.bash_profile`, or `.profile`):
  ```sh
  echo 'export VARIABLE_NAME=value' >> ~/.bashrc
  source ~/.bashrc
  ```

### 6. File System Differences

Moving files between different file systems or partitions can sometimes cause issues, although this is less common with modern systems. Here are some considerations:

- **File System Types**:
  Different file systems (e.g., ext4, NTFS, FAT32) have different capabilities and restrictions. For instance, file permissions and ownership might not be preserved when moving files between file systems with different permission models.

- **Mount Options**:
  The options used when mounting a file system can affect file execution. For example, the `noexec` option prevents execution of binaries on the mounted file system:
  ```sh
  mount | grep /usr/bin
  ```
  Ensure that `/usr/bin` is not mounted with `noexec`.

- **Inodes and File Metadata**:
  Some file systems handle file metadata differently. If the binary relies on specific metadata that’s not preserved during the move, it might not run correctly. This is rare but can happen in specialized use cases.

- **Consistency and Corruption**:
  Ensure the file system is consistent and not corrupted. Use tools like `fsck` to check and repair file system issues:
  ```sh
  sudo fsck -f /dev/sdXn  # Replace /dev/sdXn with your actual device identifier
  ```

By examining these factors—library dependencies, environment variables, and file system differences—you can identify and resolve issues that prevent your binary from executing correctly after being moved.
