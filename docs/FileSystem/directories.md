# Directories

No storage system contains a single file.
- Files are organized in directories. A directory is a collection of files.
- A directory is a node (of a tree) or a vertex of a graph that stores information
about the file contained there
- Both directories and files are saved in mass memory

## KPI (Key performance indicators)

1. Efficiency: speed in modifying the file system, i.e. deleting a file, searching a file .. 
2. Naming: how simple it is for a user to identify a file, and if the FS allows the assignment of the same name to different files
3. Grouping: grouping the files based on the content they contain from a semantic point of view. (editors, compilers, documents ..)

/
-> summerpics/
-> parent_pics/


### Directories with only one level (no subdirectories allowed)
cat/
bo/
a/
test/ 
..

## Definition of a Directory
A Directory is a list of files and pointers to the data.
key, pointer to the data block that represents the file
cat/, pointer to the file contained in cat/
bo/, poitner to the file contained in bo/
..


Key, Value
filename, inode (is an integer)
file.txt - 10257

### Directories with two levels
### Directories with three levels


### Acyclic graph directories 



In linux you cannot have hard links to directories.


## Contiguous Allocation

The file system divides the hard disk in blocks
of different dimensions. We create a partition
and then we install a file system. In linux we
have ext 4.
If I have 11 kbs of a file, in blocks of 4 kbs,
there's a phenomenon called internal segmentation.

slide 23

internal fragmentation: last block only partially used

external fragmentaton: problem, can be solved
with defragmentation. It's not good though, then
if you want to appen something to a file that had
other free memory ahead.  

first fit, worst fit, best fit: question in the exam


