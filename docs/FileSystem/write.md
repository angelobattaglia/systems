#  The `write` syscall

## File Descriptors
In UNIX‐style operating systems (Linux, BSD, macOS), every process maintains a small table of file descriptors—non-negative
integers that act as opaque handles to I/O resources (files, pipes, sockets, devices, etc.). 
By convention, the first three entries of this table are reserved:

0 → STDIN_FILENO
Standard input; by default reads from the terminal or whatever is piped in.

1 → STDOUT_FILENO
Standard output; by default writes to the terminal.

2 → STDERR_FILENO
Standard error; also writes to the terminal but typically unbuffered, for error messages.

