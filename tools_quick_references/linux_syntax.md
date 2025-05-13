| Command          | Example Usage                 | What it does                                                                                                |
|------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------|
| `pwd`            | `pwd`                         | Print Working Directory: Displays the full path of your current directory.                                    |
| `cd`             | `cd`                          | Change Directory: Navigates to the user's home directory.                                                    |
|                  | `cd <directory>`              | Navigates to the specified directory.                                                                       |
|                  | `cd ..`                       | Moves one level up in the directory hierarchy.                                                              |
|                  | `cd -`                        | Returns to the previous directory.                                                                            |
| `ls`             | `ls`                          | List: Displays the files and directories in the current directory.                                             |
|                  | `ls -l`                       | Long listing: Shows detailed information about files and directories (permissions, owner, size, date, etc.). |
|                  | `ls -a`                       | All: Shows all files and directories, including hidden ones (those starting with a dot).                     |
|                  | `ls -h`                       | Human-readable: Displays file sizes in a human-readable format (e.g., 1K, 234M, 2G).                          |
|                  | `ls -r`                       | Reverse: Lists files and directories in reverse order.                                                      |
|                  | `ls -t`                       | Sort by time: Lists files and directories sorted by modification time (newest first).                         |
| `mkdir`          | `mkdir <directory>`           | Make Directory: Creates a new directory with the specified name.                                             |
|                  | `mkdir -p <path>`             | Parents: Creates parent directories if they don't exist.                                                     |
| `rmdir`          | `rmdir <directory>`           | Remove Directory: Deletes an empty directory.                                                              |
| `rm`             | `rm <file>`                   | Remove: Deletes the specified file.                                                                         |
|                  | `rm -r <directory>`           | Recursive: Deletes a directory and its contents (files and subdirectories) - use with caution!              |
|                  | `rm -f <file>`                | Force: Forces deletion without prompting.                                                                    |
|                  | `rm -rf <directory>`          | Force and Recursive: Forces deletion of a directory and its contents without prompting - use with extreme caution! |
| `cp`             | `cp <source> <destination>`   | Copy: Copies a file or directory from the source to the destination.                                         |
|                  | `cp -r <source_dir> <dest_dir>` | Recursive: Copies a directory and its contents.                                                             |
| `mv`             | `mv <source> <destination>`   | Move: Moves a file or directory from the source to the destination. Can also be used to rename files/directories. |
| `cat`            | `cat <file>`                  | Concatenate: Displays the content of a file on the terminal.                                                |
| `less`           | `less <file>`                 | Allows you to view the content of a file page by page, with navigation.                                     |
| `head`           | `head <file>`                 | Displays the first 10 lines of a file.                                                                      |
|                  | `head -n <number> <file>`    | Displays the first specified number of lines of a file.                                                    |
| `tail`           | `tail <file>`                 | Displays the last 10 lines of a file.                                                                       |
|                  | `tail -n <number> <file>`    | Displays the last specified number of lines of a file.                                                     |
|                  | `tail -f <file>`              | Follow: Displays the last lines of a file and continues to show new lines as they are added.                 |
| `grep`           | `grep "<pattern>" <file>`     | Globally search a Regular Expression and Print: Searches for a specific pattern within a file.                |
|                  | `grep -i "<pattern>" <file>`  | Ignore case: Performs a case-insensitive search.                                                             |
|                  | `grep -r "<pattern>" <dir>`   | Recursive: Searches for a pattern within all files in a directory and its subdirectories.                  |
|                  | `grep -v "<pattern>" <file>`  | Invert match: Displays lines that do *not* contain the pattern.                                            |
| `find`           | `find <path> -name "<name>"` | Searches for files and directories with the specified name within the given path.                             |
|                  | `find <path> -type f`         | Finds only files within the given path.                                                                     |
|                  | `find <path> -type d`         | Finds only directories within the given path.                                                               |
|                  | `find <path> -size +10M`      | Finds files larger than 10 megabytes.                                                                       |
|                  | `find <path> -mtime -7`      | Finds files modified within the last 7 days.                                                                |
| `chmod`          | `chmod <permissions> <file>`  | Change mode: Modifies the permissions of a file or directory.                                               |
|                  | `chmod 755 <file>`            | Sets read, write, and execute permissions for the owner, and read and execute for group and others.          |
|                  | `chmod u+x <file>`            | Adds execute permission for the owner.                                                                      |
| `chown`          | `chown <user> <file>`         | Change owner: Changes the owner of a file or directory (requires superuser privileges).                     |
|                  | `chown <user>:<group> <file>` | Changes both the owner and the group of a file or directory.                                                |
| `sudo`           | `sudo <command>`              | Super User Do: Executes a command with superuser (root) privileges.                                          |
| `df`             | `df -h`                       | Disk Free: Displays disk space usage in a human-readable format.                                            |
| `du`             | `du -h <directory>`           | Disk Usage: Displays the disk space used by a directory and its contents in a human-readable format.        |
| `ps`             | `ps aux`                      | Process Status: Displays information about running processes.                                                 |
|                  | `ps aux | grep <process>`    | Filters the process list to show only processes matching the given name.                                    |
| `kill`           | `kill <PID>`                  | Terminates a process with the specified Process ID (PID).                                                     |
|                  | `kill -9 <PID>`               | Forces termination of a process (use as a last resort).                                                      |
| `top`            | `top`                         | Displays a dynamic real-time view of running processes and system resource usage.                             |
| `history`        | `history`                     | Displays a list of previously executed commands.                                                              |
|                  | `!n`                          | Executes the nth command in the history list.                                                               |
|                  | `!!`                          | Executes the last executed command.                                                                         |
| `man`            | `man <command>`               | Manual: Displays the manual page for a given command, providing detailed information about its usage and options. |
| `uname`          | `uname -a`                    | Prints system information.                                                                                    |
| `tar`            | `tar -cvf <archive.tar> <files>` | Create an archive: Creates a non-compressed archive of the specified files and directories.                  |
|                  | `tar -xvf <archive.tar>`      | Extract an archive: Extracts the contents of a `.tar` archive.                                               |
|                  | `tar -czvf <archive.tar.gz> <files>` | Create a gzipped archive: Creates a compressed archive.                                                    |
|                  | `tar -xzvf <archive.tar.gz>`    | Extract a gzipped archive.                                                                                  |
| `gzip`           | `gzip <file>`                 | Compresses a file, creating a `.gz` file.                                                                   |
| `gunzip`         | `gunzip <file.gz>`            | Decompresses a `.gz` file.                                                                                    |
| `zip`            | `zip <archive.zip> <files>`   | Creates a `.zip` archive.                                                                                     |
| `unzip`          | `unzip <archive.zip>`         | Extracts files from a `.zip` archive.                                                                        |
| `ssh`            | `ssh <user>@<host>`           | Secure Shell: Establishes a secure connection to a remote server.                                            |
| `scp`            | `scp <user>@<host>:<remote_file> <local_file>` | Secure Copy: Securely copies files between local and remote systems.                                      |
|                  | `scp <local_file> <user>@<host>:<remote_path>` |                                                                                             |
| `wget`           | `wget <url>`                  | Web Get: Downloads files from the internet.                                                                   |
| `curl`           | `curl <url>`                  | Client URL: Transfers data from or to a server (often used for web requests).                               |
| `apt update`     | `sudo apt update`             | Updates the package lists for upgrades and new package installations (Debian/Ubuntu based).                    |
| `apt upgrade`    | `sudo apt upgrade`            | Upgrades the installed packages to their newest versions (Debian/Ubuntu based).                               |
| `yum update`     | `sudo yum update`             | Updates all packages to the latest version (Red Hat/CentOS based).                                             |
| `yum install`    | `sudo yum install <package>`  | Installs a new package (Red Hat/CentOS based).                                                                |
| `apt install`    | `sudo apt install <package>`  | Installs a new package (Debian/Ubuntu based).                                                                |
| `systemctl`      | `sudo systemctl start <service>` | Starts a system service (e.g., apache2, nginx).                                                              |
|                  | `sudo systemctl stop <service>`  | Stops a system service.                                                                                     |
|                  | `sudo systemctl restart <service>` | Restarts a system service.                                                                                  |
|                  | `sudo systemctl status <service>` | Shows the status of a system service.                                                                     |
| `ifconfig`       | `ifconfig`                    | Configures or displays network interface parameters (older systems).                                           |
| `ip addr`        | `ip addr show`                | Displays network interface addresses and properties (newer systems).                                         |
| `netstat`        | `netstat -tuln`               | Displays network connections, listening ports, Ethernet statistics, the IP routing table, IPv4 statistics (older systems). |
| `ss`             | `ss -tuln`                    | Socket Statistics: Displays socket-related information (newer systems, often preferred over netstat).          |
| `exit`           | `exit`                        | Closes the current terminal session.                                                                          |