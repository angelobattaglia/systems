## Concurrent v. Parallel
- Concurrent programming is about structure and management: handling multiple tasks by interleaving execution to improve responsiveness.

- Parallel computing is about performance and speed: dividing computations to physically run them simultaneously across multiple processors.

### **1. UID and GID**

- **UID (User ID):** Identifies a user uniquely on a Unix/Linux system.
- **GID (Group ID):** Identifies the user's primary group on the system.

The functions:
```c
uid_t getuid();
gid_t getgid();
```
These retrieve the real user ID and real group ID of the process, respectively.  
The "real" IDs typically represent the user who initiated the process.

---

### **2. Effective UID and Effective GID**

In addition to the real IDs, processes have an **Effective UID (EUID)** and **Effective GID (EGID)**.

- **Effective UID (EUID)** determines the permissions for file access and actions the process can perform.
- **Effective GID (EGID)** affects group-level permissions.

The functions:
```c
uid_t geteuid();
gid_t getegid();
```
retrieve the effective user and group IDs, respectively.

---

### **3. Practical Example (passwd Command)**

A typical example of this mechanism is the `passwd` command:

- Changing a user's password involves modifying sensitive system files, which typically only root can access.
- The `passwd` command needs temporary elevated permissions to perform this task, even if executed by a regular user.
- Therefore, `passwd` has the **SetUID bit** set. When executed, the operating system temporarily grants the command the **Effective UID** of the file's owner (usually root).
- Consequently, even though the Real UID remains the user running the command, the Effective UID is temporarily elevated to root to complete the task.

**Example scenario:**
- Normal user executes: `passwd`
- System recognizes the SetUID bit on `/usr/bin/passwd`
<!-- - Temporarily elevates the command’s Effective UID to root. -->
- Command runs with root permissions to change the password.
- After execution, privileges are dropped back to normal user levels.

---

### **In Short:**
- **UID/GID** → identity of the user/process.
- **Effective UID/GID** → permissions a process uses to interact with system resources.
- **SetUID/SetGID bit** → allows temporary privilege elevation, crucial for secure system operations.

This mechanism is key to balancing security and usability in Unix/Linux systems.

### Operative Example

This output
```shell
Process ID (PID): 22153
Parent Process ID (PPID): 21767
User ID (UID): 1000
Group ID (GID): 1000
Effective User ID (EUID): 1000
Effective Group ID (EGID): 1000
```

means
```text
Explanation of your output:
PID (Process ID):
22153 is the unique identifier assigned by the system to your running process.

PPID (Parent Process ID):
21767 identifies the parent process which launched your current program (often your shell, IDE, or terminal).

User ID (UID):
1000 usually indicates a standard, non-root user account (the first user account created in a Unix/Linux system is commonly assigned UID 1000).

Group ID (GID):
1000 is your default user group, typically matching your UID.

Effective User ID (EUID) and Effective Group ID (EGID):
Both 1000, indicating that no special privileges or changes to identity have been made to your program at this point.
```

