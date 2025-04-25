# POSIX system calls for File Handling, part IV

The **POSIX system calls** `open()`, `read()`, `write()`, and `close()` are low-level file operations used in C programming to interact with files. They provide direct interaction with the operating system and are included in `<fcntl.h>` and `<unistd.h>`. Below is an explanation of how to use each of these system calls, with examples.

### 1. **`open()`**
The `open()` system call opens a file for reading, writing, or both. It returns a **file descriptor** (an integer) that is used in subsequent file operations.

#### **Syntax**
```c
#include <fcntl.h>
#include <unistd.h>

int open(const char *pathname, int flags, mode_t mode);
```

- **`pathname`**: The file to open (e.g., `"file.txt"`).
- **`flags`**: Specifies the access mode:
  - `O_RDONLY`: Open for reading.
  - `O_WRONLY`: Open for writing.
  - `O_RDWR`: Open for reading and writing.
  - Add flags like `O_CREAT` (create file if it doesn’t exist) or `O_APPEND` (append mode).
- **`mode`**: File permissions (only used if creating a file, e.g., `0644` for `rw-r--r--`).

#### **Example**
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    int fd = open("example.txt", O_WRONLY | O_CREAT, 0644);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    // Use `fd` for further operations
    close(fd);
    return 0;
}
```

---

### 2. **`read()`**
The `read()` system call reads data from a file into a buffer.

#### **Syntax**
```c
ssize_t read(int fd, void *buf, size_t count);
```

- **`fd`**: File descriptor returned by `open()`.
- **`buf`**: Buffer to store the data.
- **`count`**: Number of bytes to read.
- **Returns**: Number of bytes actually read, or `0` if end-of-file (EOF) is reached.

#### **Example**
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    char buffer[128];
    int fd = open("example.txt", O_RDONLY);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    ssize_t bytesRead = read(fd, buffer, sizeof(buffer) - 1);
    if (bytesRead == -1) {
        perror("read");
        close(fd);
        return 1;
    }
    buffer[bytesRead] = '\0'; // Null-terminate the buffer
    printf("Read: %s\n", buffer);
    close(fd);
    return 0;
}
```

---

### 3. **`write()`**
The `write()` system call writes data from a buffer to a file.

#### **Syntax**
```c
ssize_t write(int fd, const void *buf, size_t count);
```

- **`fd`**: File descriptor returned by `open()`.
- **`buf`**: Buffer containing the data to write.
- **`count`**: Number of bytes to write.
- **Returns**: Number of bytes written.

#### **Example**
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    const char *text = "Hello, world!\n";
    int fd = open("example.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    ssize_t bytesWritten = write(fd, text, 14);
    if (bytesWritten == -1) {
        perror("write");
        close(fd);
        return 1;
    }
    printf("Wrote %zd bytes\n", bytesWritten);
    close(fd);
    return 0;
}
```

### 4. **`close()`**
The `close()` system call closes an open file descriptor, releasing the resources associated with it.

#### **Syntax**
```c
int close(int fd);
```

- **`fd`**: File descriptor to close.
- **Returns**: `0` on success, or `-1` on failure.

#### **Example**
```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    int fd = open("example.txt", O_RDONLY);
    if (fd == -1) {
        perror("open");
        return 1;
    }
    // Perform file operations...
    if (close(fd) == -1) {
        perror("close");
        return 1;
    }
    return 0;
}
```

---

### Complete Example: File Copy Program
Here's a small program that copies the content of one file to another:

```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>

int main() {
    char buffer[1024];
    ssize_t bytesRead, bytesWritten;

    // Open source file
    int source = open("source.txt", O_RDONLY);
    if (source == -1) {
        perror("open source");
        return 1;
    }

    // Open destination file
    int dest = open("dest.txt", O_WRONLY | O_CREAT | O_TRUNC, 0644);
    if (dest == -1) {
        perror("open dest");
        close(source);
        return 1;
    }

    // Read and write loop
    while ((bytesRead = read(source, buffer, sizeof(buffer))) > 0) {
        bytesWritten = write(dest, buffer, bytesRead);
        if (bytesWritten != bytesRead) {
            perror("write");
            close(source);
            close(dest);
            return 1;
        }
    }
    if (bytesRead == -1) {
        perror("read");
    }

    // Close files
    close(source);
    close(dest);
    return 0;
}
```

---

### Notes:
- Always check the return values of these calls for errors.
- Errors can be inspected using `errno` and printed with `perror()` or `strerror()`.
- File permissions (e.g., `0644`) are written in octal.

Let me know if you have further questions!