# POSIX threads or `pthreads`

A thread is a function that is executed in
concurrency with the main thread
A process with multiple threads = a set of independently executing
functions that share the process resources

The Pthreads library allows
1. Creating and manipulating threads
2. Synchronizing threads
3. Protection of resources shared by threads
4. Thread scheduling
5. Destroying thread

It defines more than 60 functions
1. All functions have a `pthread_*` prefix
2. `pthread_equal`, `pthread_self`, `pthread_create`, `pthread_exit`, `pthread_join`, `pthread_cancel`, `pthread_detach`

The Pthread system calls are defined in
1. pthreads.h
2. It is necessary to remember
3. To insert in the .c files
4. `#include <pthread.h>`
5. Compile your program linking the pthread library
6. ```gcc -Wall -g -o <exeName> <file.c> -lpthread```

A thread is uniquely identified
1. By a type identifier pthread_t
2. Similar to the PID of a process (pid_t)
3. The type pthread_t is opaque
4. Its definition is implementation dependent
5. Can be used only by functions specifically defined in Pthreads
6. It is not possible compare directly two identifiers or print their values
7. It has meaning only within the process where the thread is executed
8. Remember that the PID is global within the system

