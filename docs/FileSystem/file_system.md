Information Storage

Information is preserved over extended periods, remaining unaffected by:
1. Program or process termination.
2. Loss of power supply or other disruptions.
3. Logical Perspective on Files

A file is:
  1. A structured collection of related data, encompassing elements such as numbers, text, images, and more.
  2. Data stored on electronic devices using standardized encoding systems.
  3. Represented in a contiguous address space within the storage medium.


ASCII encoding
De-facto standard
➢ ASCII, American Standard
Code for Information Interchange
128 total characters
32 not printable
96 printable
▪ Originally based on the English alphabet
▪ 128 characters are coded in 7-bit (binary numbers)
➢ Extended ASCII (or high ASCII)
▪ Extension of ASCII to 8-bit and 255 characters
▪ Several versions exist
● ISO 8859-1 (ISO Latin-1), ISO 8859-2 (Eastern
European languages), ISO 8859-5 for Cyrillic
languages, etc.


Unicode encoding
❖ Industrial standard that includes the alphabets for
any existing writing system
➢ It contains more 110,000 characters
➢ It includes more than 100 sets of symbols
❖ Several implementations exist
➢ UCS (Universal Character Set)
➢ UTF (Unicode Tranformation Format)
▪ UTF-8, groups of 8 bits size (1, 2, 3 or 4 groups)
● ASCII coded in the first 8 bits
▪ UTF-16, groups of 16 bits size (1 or 2 groups)
▪ UTF-32, groups of 32 bits size (fixed length)


## Textual and binary files
A file is basically a sequence of bytes written one
after the other. Each byte includes 8 bits, with possible values 0 or 1.
As a consequence all files are binary. 
Normally we can distinguish between:
1. Textual files (or ASCII) (C sources, C++ sources etc..)
- Binary files (Executables, Word, Excel ..)
### Remark:
The UNIX/Linux kernel does not distinguish
between binary and textual files.


## Textual files (or ASCII) (to give to chatGPT for better explaination)
❖ Files consisting of data encoded in ASCII
➢ Sequence of 0 and 1, which (in groups of 8 bit)
codify ASCII symbols
❖ Textual files are usually “line-oriented”
➢ Newline: go to the next line
▪ UNIX/Linux and Mac OSX
● Newline = 1 character
● Line Feed (go to next line, LF, 10 10)
▪ Windows
● Newline = 2 characters
● Line Feed (go to next line, LF, 10 10)
+ Carriage Return (go to beginning of the line, CR, 13 10)


## Binary Files
❖ A binary file is a sequence of 0 and 1, not “byte-oriented”, meaning it's bit by bit, not byte by byte
❖ The smallest unit that can be read/write is the bit
➢ Non easy the management of the single bit
➢ They usually include every possible sequence of 8
bits, which do not necessarily correspond to
printable characters, new-line, etc.


## Why are binary files used?
❖ Advantages
➢ Compactness (smaller average dimension)
▪ Examples: Number 10000010 occupies 6 characters,
(i.e., 6 bytes) in the Text/ASCII format, and 4 bytes
if coded in an integer (short)
➢ Ease of editing the file
▪ An integer always occupies the same space
➢ Ease of positioning on the file
▪ Fixed record structure
❖ Drawbacks
➢ Limited portability
➢ Impossibility to use a standard editor


## Why are binary files used?
❖ Advantages
➢ Compactness (smaller average dimension)
▪ Examples: Number 10000010 occupies 6 characters,
(i.e., 6 bytes) in the Text/ASCII format, and 4 bytes
if coded in an integer (short)
➢ Ease of editing the file
▪ An integer always occupies the same space
➢ Ease of positioning on the file
▪ Fixed record structure
❖ Drawbacks
➢ Limited portability
➢ Impossibility to use a standard editor


## Serialization

**Definition:**  
Serialization is the process of converting a data structure
(e.g., a `struct` in C) into a format that can be stored
or transmitted and later reconstructed in its original form. 

- **Purpose:**  
  - Allows a data structure to be stored (e.g., in a file or database)
  or transmitted (e.g., over a network) as a single entity.  
  - The serialized data is read and reconstructed according to the same
  serialization format, ensuring the original structure is replicated.

**Features:**  
  - Most programming languages support serialization using read/write 
    (R/W) operations on files. Examples include:  
  - Java  
  - Python  
  - Objective-C  
  - Ruby  

---

### **Example of Serialization in C**

**Structure Definition:**
```c
struct mys {
    int id;
    long int rn;
    char n[L], c[L];
    int mark;
} s;
```

**Serialization Formats:**  
1. **Binary Serialization:**  
   - The structure is stored as a sequence of bytes.  
   - The interpretation depends on the encoding scheme (e.g., ASCII on 8 bits or Unicode on 16 bits).  
   - The file size is compact but not human-readable.

2. **Text Serialization:**  
   - Each field is stored as text, using character encoding (e.g., ASCII on 8 bits).  
   - Example:  
     ```
     1 100000 Romano Antonio 25
     ```
   - Larger file size compared to binary serialization but more human-readable.  

---

### **Notes:**
- Serialization formats affect file size and readability:  
  - **Binary formats** are compact but require decoding.  
  - **Text formats** are larger but easy to interpret.  
- Incorrect encoding or decoding can result in malformed data (e.g., corrupted text with unrecognized characters).

--- 

## I/O with the C standard library
I/O operations with ANSI C can be performed
through different categories of functions
1. Character by character
2. Row by row
3. Formatted I/O
4. Binary I/O
[Read examples](https://www.skenz.it/cs/c_language/file_reading_1)
[Write examples](https://www.skenz.it/cs/c_language/file_writing_1)
[Binary I/O examples](https://www.skenz.it/cs/c_language/write_and_read_a_binary_file)

## ISO C Standard Library
Standard I/O is “fully buffered”
➢ The I/O operation is performed only when the I/O
buffer is full
➢ The “flush” operation indicates the actual write of
the buffer to the I/O
```c
#include <stdio.h>
void setbuf (FILE *fp, char *buf);
int fflush (FILE *fp);
```
Standard error is never buffered.
For concurrent processes, use:
```c
setbuf (stdout, 0);
fflush (stdout);
```

## Open and close a file
Here's how to open and close a file
```c
#include <stdio.h>
FILE *fopen (char *path, char *type);
FILE *fclose (FILE *fp);
```
### Access methods
- r, rb, w, wb, a, ab r+, r+b, etc.
- The UNIX kernel does not make any difference
between textual files (ASCII) and binary files
- The “b” option has no effect, e.g. “r”==“rb”, “w”==“wb”, etc.

## I/O character by character
```c
#include <stdio.h>
int getc (FILE *fp);
int fgetc (FILE *fp);
int putc (int c, FILE *fp);
int fputc (int c, FILE *fp);
```
❖ Returned values
➢ A character on success
➢ EOF on error, or when the end of the file is
reached
❖ The function
➢ getchar is equivalent to getc (stdin)
➢ putchar is equivalent to putc (c, stdout)

## I/O row by row
```c
#include <stdio.h>
char *gets (char *buf);
char *fgets (char *buf, int n, FILE *fp);
int puts (char *buf);
int fputs (char *buf, FILE *fp);
```
❖ Returned values
➢ buf (gets/fgets), or a non-negative value
(puts/fputs) in the case of success
➢ NULL (gets/fgets), or EOF for errors or when the
end of file is reached (puts/fputs)
❖ Lines must be delimited by "new-line"

## Formatted I/O
```c
#include <stdio.h>
int scanf (char format, ...);
int fscanf (FILE *fp, char format, ...);
int printf (char format, ...);
int fprintf (FILE *fp, char format, ...);
```
❖ High flexibility in data manipulation
➢ Formats (characters, integers, reals, etc.)
➢ Conversions

## Binary I/O
```c
#include <stdio.h>
size_t fread (void *ptr, size_t size,
size_t nObj, FILE *fp);

size_t fwrite (void *ptr, size_t size,
size_t nObj, FILE *fp);
```
Each I/O operation (single) operates on an aggregate object of specific size
aggregate object of specific size
all the fields of the struct
➢ With gets/puts it is not possible, because both
would terminate on NULL bytes or new-lines
❖ Returned values
➢ Number of objects written/read
➢ If the returned value does not correspond to the ferror and feof can be parameter nObj used to distinguish 
▪ An error has occurred between the two cases 
▪ The end of file has been reached
❖ Often used to manage binary files
➢ serialized R/W (single operation for the whole struct)
➢ Potential problems in managing different architectures
▪ Data format compatibility (e.g., integers, reals, etc.)
▪ Different offsets for the fields of the struct

## POSIX Standard Library of sys calls

I/O in UNIX can be entirely performed with only 5 functions
- open, read, write, lseek, close
❖ This type of access
➢ Is part of POSIX and of the Single UNIX Specification, but not of ISO C
➢ It is normally defined with the term "unbuffered I/O", in the sense that each read or write operation corresponds to a system call

### System call open()

❖ In the UNIX kernel a "file descriptor" is a non-negative integer
❖ Conventionally (also for shells)
➢ Standard input
▪ 0 = STDIN_FILENO
➢ Standard output
▪ 1 = STDOUT_FILENO
➢ Standard error
▪ 2 = STDERR_FILENO
These descriptors are defined
in the headers file unistd.h

### System call open()

#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int open (const char *path, int flags);
int open (const char *path, int flags,
mode_t mode);
❖ It opens a file defining the permissions
❖ Returned values
➢ The descriptor of the file on success
➢ -1 on error

### System call open()

❖ It can have 2 or 3 parameters
➢ The mode parameter is optional
❖ Path indicates the file to open
❖ Flags has multiple options
int open (
const char *path,
int flags,
mode_t mode
);
➢ Can be obtained with the OR bit-by-bit of
constants defined in the header file fcntl.h
➢ One of the following three constants is mandatory
▪ O_RDONLY
▪ O_WRONLY
▪ O_RDWR
open for read-only access
open for write-only access
open for read-write access

### System call open()

➢ Optional constants
▪ O_CREAT
▪ O_EXCL
▪ O_TRUNC
▪ O_APPEND
▪ O_SYNC
▪ ...
int open (
const char *path,
int flags,
mode_t mode
);
creates the files if not exist
error if O_CREAT is set and the file
exists
remove the content of the file
append to the file
each write waits that the physical
write operation is finished
before continuing

### System call open()

➢ Optional constants
▪ O_CREAT
▪ O_EXCL
▪ O_TRUNC
▪ O_APPEND
▪ O_SYNC
▪ ...
int open (
const char *path,
int flags,
mode_t mode
);
creates the files if not exist
error if O_CREAT is set and the file
exists
remove the content of the file
append to the file
each write waits that the physical
write operation is finished
before continuing

### System call read()
#include <unistd.h>
int read (int fd, void *buf, size_t nbytes);
❖ Read from file fd a number of bytes equal to
nbytes, storing them in buf
❖ Returned values
➢ number of read bytes on success
➢ -1 on error
➢ 0 in the case of EOF

### System call read()
#include <unistd.h>
int read (int fd, void *buf, size_t nbytes);
❖ The returned value is lower that nbytes
➢ If the end of the file is reached before nbytes
bytes have been read
➢ If the pipe you are reading from does not contain
nbytes bytes

### System call write()
#include <unistd.h>
int write (int fd, void *buf, size_t nbytes);
❖ Write nbytes bytes from buf in the file identified
by descriptor fd
❖ Returned values
➢ The number of written bytes in the case of
success, i.e., normally nbytes
➢ -1 on error

### System call write()
#include <unistd.h>
int write (int fd, void *buf, size_t nbytes);
❖ Remark
➢ write writes on the system buffer, not on the disk
▪ fd = open (file, O_WRONLY | O_SYNC);
➢ O_SYNC forces the sync of the buffers, but only
for ext2 file systems

Examples: File R/W
float data[10];
if ( write(fd, data, 10*sizeof(float))==(-1) ) {
fprintf (stderr, "Error: Write %d).\n", n);
}
}
writing of the vector data (of
float)
struct {
char name[L];
int n;
float avg;
} item;
if ( write(fd,&item,sizeof(item)))==(-1) ) {
fprintf (stderr, "Error: Write %d).\n", n);
}
}
Writing of the serialized struct
item (with 3 fields)

### System call lseek()
#include <unistd.h>
off_t lseek (int fd, off_t offset, int whence);
❖ The current position of the file offset is
associated to each file
➢ The system call lseek assigns the value offset to
the file offset
➢ The offset value is expressed in bytes

### System call lseek()
#include <unistd.h>
off_t lseek (int fd, off_t offset, int whence);
❖ whence specifies the interpretation of offset
➢ If whence==SEEK_SET
▪ The offset is evaluated from the beginning of the file
➢ If whence==SEEK_CUR
▪ The offset is evaluated from the current position
➢ If whence==SEEK_END
▪ The offset is evaluated from the end of the file
The value of offset
can be positive or
negative
It is possible to leave
"holes" in a file
(filled with zeros)

### System call lseek()
#include <unistd.h>
off_t lseek (int fd, off_t offset, int whence);
❖ Returned values
➢ new offset on success
➢ -1 on error

### System call close()
#include <unistd.h>
int close (int fd);
❖ Returned values
➢ 0 on success
➢ -1 on error
❖ All the open files are closed automatically when
the process terminates

### Example: File R/W
```c
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#define BUFFSIZE 4096
int main(void) {
    int nR, nW, fdR, fdW;
    char buf[BUFFSIZE];
    fdR = open (argv[1], O_RDONLY);
    fdW = open (argv[2], O_WRONLY | O_CREAT | O_TRUNC,
    S_IRUSR | S_IWUSR);
    if ( fdR==(-1) || fdW==(-1) ) {
        fprintf (stdout, “Error Opening a File.\n“);
    exit (1);
}
```

### Example : File R/W
while ( (nR = read (fdR, buf, BUFFSIZE)) > 0 ) {
nW = write (fdW, buf, nR);
if ( nR!=nW )
fprintf (stderr,
"Error: Read %d, Write %d).\n", nR, nW);
}
if ( nR < 0 )
fprintf (stderr, "Write Error.\n");
close (fdR);
close (fdW);
exit(0);
}
Error check on the last
reading operation
This program works indifferently on text and
binary files

