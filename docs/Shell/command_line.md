# Commands

## The *diff* command

## Hard Links and Soft Links

The `ln` command allows to create *hard* and *soft* links.

## Copy a directory tree recursively

```shell
cp -r ~/soEx01 ~/soEx01backup # copy it recursively
rm -r ~/soEx01 # remove it recursively
```

## wc (word count) command

`wc` prints the number of *rows*, *words*, and *characters* of the file passed as argument, respectively
```shell
wc lab01e01in.txt 
100  467 3114 lab01e01in.txt
```

## The *history*

Prints the last commands executed in the terminal
```shell
history
```

# Permission Management

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
