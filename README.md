## Виконала самостійно Слобожан Софія РПЗ-23А
# Laboratory Work Report №10
ыфв
## Topic: Changing File Ownership and Access Rights in Linux. Special Directories and Files in Linux

## Objective:
To acquire practical skills in working with the Bash shell. To get acquainted with basic actions when changing file ownership and access rights. To get acquainted with special directories and files in Linux.

## Preliminary Preparation Tasks

### Glossary of Terms
1. **File Ownership** - The ownership of a file.
2. **User ID (UID)** - User identifier.
3. **Group ID (GID)** - Group identifier.
4. **Permissions** - Access rights.
5. **Setuid** - Special permission allowing execution of a file with the rights of the owner.
6. **Setgid** - Special permission allowing execution of a file with the rights of the group.
7. **Sticky Bit** - Special permission restricting deletion of files in shared directories.
8. **Command** - Instruction executed in the shell.
9. **Terminal** - Command-line interface.
10. **Shell** - Command interpreter.

### Answers to the Preliminary Questions
1. **What is the purpose of the `id` command?**  
   The `id` command is used to display user information including UID, GID, and group memberships.

2. **How to check the access rights of a file owner?**  
   Use the `ls -l` command to display detailed information about files including access rights.

3. **How to change the group owner of a file?**  
   Use `chgrp group_name file_name` to change the group owner of a file.

4. **How to check the file type in the terminal? Provide examples for different file types.**  
   Use the `file file_name` command. Examples:  
   - `file script.sh` - shows the file is a script  
   - `file document.txt` - shows it is a text file  
   - `file image.png` - shows it is an image file

5. **What are the uses of Setuid and Setgid permissions?**  
   Setuid allows running an executable with the rights of the file owner, useful for system commands needing elevated privileges. Setgid allows running an executable with the rights of the file's group, which helps in shared resource access.

6. **What is the purpose of the Sticky Bit in the system? Provide examples when it is advisable to use it.**  
   Sticky Bit prevents users from deleting files owned by others in shared directories. For example, in `/tmp` many users share files; Sticky Bit allows only file owners to delete their files.

## Work Progress

### Commands and Their Purpose
| Command Name | Purpose and Functionality |
|--------------|---------------------------|
| `id`         | Displays UID, GID, and groups of the current user. |
| `ls -l`      | Shows detailed file information including permissions. |
| `chgrp`      | Changes the group ownership of a file. |
| `file`       | Determines the file type. |
| `chmod`      | Changes file and directory permissions. |
| `newgrp`     | Changes the current primary group of the user. |
| `touch`      | Creates a new file or updates a file's timestamp. |

### Practical Tasks

1. **Create three new users:**
   ```bash
   sudo adduser user1
   sudo adduser user2
   sudo adduser user3
   ```

2. **Create a new user group and add two of the three created users:**
   ```bash
   sudo groupadd mygroup
   sudo usermod -aG mygroup user1
   sudo usermod -aG mygroup user2
   ```

3. **Create a new file accessible for reading, editing, and execution by the owner:**
   ```bash
   touch myscript.sh
   chmod 700 myscript.sh
   ```

4. **Grant group members permission to read and execute (no write access) the file:**
   ```bash
   chmod 750 myscript.sh
   ```

5. **Create directories with different access permissions:**
   ```bash
   mkdir shared_dir
   chmod 770 shared_dir

   mkdir private_dir
   chmod 700 private_dir
   ```

6. **Create an empty file and change its permissions:**
   ```bash
   touch emptyfile
   chmod 000 emptyfile
   ```  
   Observations on changes when using `chmod 4 emptyfile` or `chmod 44 emptyfile` reflect how `chmod` interprets numeric values (single digit sets owner permissions only, two digits set owner and group permissions).

7. **Create a directory where all files belong to your group and can only be deleted by their creator:**
   ```bash
   mkdir /home/team
   chown :mygroup /home/team
   chmod 2770 /home/team
   ```

8. **Create for each user one new file, then create hard and symbolic links to it:**
   ```bash
   touch original_file
   ln original_file hard_link
   ln -s original_file symbolic_link
   ```

9. **Test file viewing and deletion by other users and draw conclusions.**

### Control Questions

1. **Examples of changing access rights using the symbolic method:**
   - To add execute permission for the owner:
     ```bash
     chmod u+x file.txt
     ```
   - To remove write permission for the group:
     ```bash
     chmod g-w file.txt
     ```
   - To add read permission for others:
     ```bash
     chmod o+r file.txt
     ```

2. **Examples of changing access rights using the numeric (octal) method:**
   - To set permissions to read, write, and execute for the owner, and read and execute for the group and others:
     ```bash
     chmod 755 file.txt
     ```
   - To set permissions to read and write for the owner, and read for the group and others:
     ```bash
     chmod 644 file.txt
     ```

3. **Purpose of the `umask` command:**
   The `umask` command sets the default file creation mask, which determines the default permissions for newly created files and directories. It effectively subtracts permissions from the default values.

4. **Comparison of hard links and symbolic links:**
   - **Hard Links:**
     - Point to the same inode as the original file.
     - Are indistinguishable from the original file.
     - Cannot cross filesystem boundaries.
     - If the original file is deleted, the hard link remains accessible.
   - **Symbolic Links:**
     - Are separate files that point to the original file's name.
     - Can cross filesystem boundaries.
     - If the original file is deleted, the symbolic link becomes broken (dangling).

5. **Can a file be executed if it has execute permissions but no read permissions (--x)? Explain.**
   Generally, a file cannot be executed without read permissions because the operating system needs to read the file's contents to execute it. However, in some cases, certain types of executables may run without read permissions, but this is not typical behavior.

6. **If we change access rights and permissions in the current session, will they be saved in the next session?**
   Yes, changes to file permissions are persistent and will remain in effect across sessions until explicitly changed again.

7. **Is there a default pattern for permissions when creating new files? How can default permissions be changed?**
   Yes, the default permissions for new files are determined by the `umask` value. You can change the default permissions by using the `umask` command followed by the desired mask value.

8. **How to create a hard link? In what situations are they advisable to use?**
   To create a hard link, use the following command:
   ```bash
   ln original_file hard_link
   ```
   Hard links are advisable when you want multiple filenames to refer to the same data on the same filesystem, ensuring that the data remains accessible even if one of the links is deleted.

9. **How to create a symbolic link? In what situations are they advisable to use?**
   To create a symbolic link, use the following command:
   ```bash
   ln -s original_file symbolic_link
   ```
   Symbolic links are advisable when you need to create shortcuts to files or directories, especially across different filesystems, or when you want to reference a file that may change location.

10. **Imagine a program needs to create a one-time temporary file that will no longer be needed after the program closes. What is the appropriate directory for creating this file?**
    The appropriate directory for creating a temporary file is `/tmp`, as it is designated for temporary files and is typically cleared on system reboot.

11. **There is an original file and two links created for it - a symbolic link and a hard link. What happens to the other files if you delete:**
    - **The original file:** The hard link remains accessible because it points to the same inode. The symbolic link becomes broken (dangling) since it points to the original file's name, which no longer exists.
    - **The symbolic link:** The original file and the hard link remain intact and accessible.
    - **The hard link:** The original file remains accessible, and the symbolic link remains intact but will point to the same inode as the original file. If the original file is deleted, the hard link still provides access to the data.
## Conclusions

During this laboratory work, practical skills in managing file ownership and access rights in Linux were acquired. Understanding and using commands related to ownership, groups, and permissions is essential for system security and resource management. Special permissions like Setuid, Setgid, and the Sticky Bit provide additional control needed in multi-user environments. This knowledge allows efficient sharing and protection of files and directories in Linux systems.
