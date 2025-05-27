# Permission Management

Managing permissions in a Linux system is a crucial task for ensuring security and proper access control. This tutorial will guide you through verifying your username and group, exploring the file system structure, understanding directory access rights, and modifying permissions using both numeric (octal) and symbolic methods.

### Verifying Your Username and Group

To check your username and group, use the following commands:

- **`who` Command**: Displays your login name and other session information.
  ```shell
  who
  ```

- **`groups` Command**: Shows the groups you belong to.
  ```shell
  groups
  ```

**Is it possible to modify them?**  
Yes, it is possible to modify your username or group if you have the required administrative rights. 
For example, a user with root privileges can use the `usermod` command to change a username or group.

Example:
```shell
sudo usermod -l new_username old_username
sudo usermod -g new_group username
```

### Checking Permissions on Directories
To check the read, write, and execute permissions on various directories, use the following command:
```shell
ls -l /home
```

This will show you a detailed list of directories in /home with their permissions, owners, and groups. For example:
```shell
drwxr-xr-x  2 user1 user1 4096 Dec  4 10:00 user1
```

- The first column shows the permissions (drwxr-xr-x).
- The second column shows the owner (user1).
- The third column shows the group (user1).

To see the permissions for a specific directory or file, use:
```shell
ls -l /home/user1
```
This will list the permissions, owner, and group for files and directories within /home/user1.


### Modifying Permissions on Directories

You can change permissions using the `chmod` command.

#### Symbolic Permissions

To modify access rights using symbolic notation, you can add or remove specific rights for the user (u), group (g), and others (o). For example:

* **Remove read permission from others**:

  ```bash
  chmod o-r /home/user1
  ```

* **Add write permission for the user**:

  ```bash
  chmod u+w /home/user1
  ```

#### Numeric (Octal) Permissions

You can also use numeric values to modify permissions. The octal notation corresponds to the following:

* `r` (read) = 4
* `w` (write) = 2
* `x` (execute) = 1

Permissions are represented as a three-digit number: `rwxr-xr-x` becomes `755`.

Example:

* **Setting `755` permissions** (read, write, execute for the owner, and read and execute for group and others):

  ```bash
  chmod 755 /home/user1
  ```

---

### Removing Read or Execution Rights for a Directory

If you want to remove read or execute permissions for a directory, use:

#### Using Symbolic Notation:

* **Remove read permission from everyone**:

  ```bash
  chmod a-r /home/user1
  ```

* **Remove execute permission for group**:

  ```bash
  chmod g-x /home/user1
  ```

#### Using Octal Notation:

* **Remove execute permission** (equivalent to `rwxr-xr-x` to `rw-r--r--`):

  ```bash
  chmod 644 /home/user1
  ```

---

### Recursively Modifying Permissions

To modify the permissions of all files and subdirectories within a directory tree, use the `-R` (recursive) option.

For example, to remove read access for all users (owner, group, and others) from the directory `osEx01backup`, use:

```bash
chmod -R a-r /path/to/osEx01backup
```

This will apply the changes to all files and subdirectories inside `osEx01backup`.

---

### Summary of Common Commands

* **View current username and groups**:

  ```bash
  who
  groups
  ```

* **View the structure of the `/home` directory**:

  ```bash
  ls /home
  ```

* **Check detailed directory permissions**:

  ```bash
  ls -l /home
  ```

* **Modify permissions** (symbolic):

  ```bash
  chmod u+x /home/user1   # Add execute permission to user
  chmod g-w /home/user1   # Remove write permission from group
  chmod a-r /home/user1   # Remove read permission from everyone
  ```

* **Modify permissions** (numeric):

  ```bash
  chmod 755 /home/user1   # Set rwx for owner, rx for group and others
  chmod 644 /home/user1   # Set rw- for owner, r-- for group and others
  ```

* **Recursively modify permissions**:

  ```bash
  chmod -R a-r /path/to/osEx01backup  # Remove read permission for all in osEx01backup
  ```
