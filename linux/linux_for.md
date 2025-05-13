# Linux Cybersecurity Investigation Artifacts

This document provides a list of important files, log locations, and forensic artifacts relevant to cybersecurity investigations on Linux operating systems.

| Artifact Type          | Path/Location                                       | Description                                                                 |
|------------------------|-------------------------------------------------------|-----------------------------------------------------------------------------|
| **Log Files** |                                                       |                                                                             |
| System Log (syslog/rsyslog/journald) | `/var/log/syslog`, `/var/log/messages`, `journalctl` | General system messages, service startups, errors, kernel messages.       |
| Authentication Log     | `/var/log/auth.log`, `/var/log/secure`                | Login attempts (successful and failed), sudo usage, SSH activity.           |
| Kernel Log             | `/var/log/kern.log`, `dmesg`                          | Kernel-related messages, hardware events, driver information.               |
| Cron Log               | `/var/log/cron`, `/var/log/syslog` (may vary)         | Information about scheduled tasks (cron jobs) execution.                    |
| Web Server Logs (e.g., Apache, Nginx) | `/var/log/apache2/access.log`, `/var/log/nginx/access.log` (and `error.log` equivalents) | Records HTTP requests, client IPs, user agents, requested resources.      |
| FTP Server Logs        | `/var/log/vsftpd.log`, `/var/log/proftpd/` (varies by daemon) | FTP connection attempts, file transfers, commands executed.                 |
| Mail Server Logs       | `/var/log/mail.log`, `/var/log/maillog` (varies by MTA) | Email delivery information, sender/recipient, connection attempts.          |
| Auditd Logs            | `/var/log/audit/audit.log`                            | Detailed system call auditing, file access, security events (if configured). |
| Lastlog                | `/var/log/lastlog` (binary file, use `lastlog` command) | Information about the last login time for each user.                      |
| Wtmp                   | `/var/log/wtmp` (binary file, use `last` command)     | Historical login and logout information for all users.                      |
| Btmp                   | `/var/log/btmp` (binary file, use `lastb` command)    | Records failed login attempts.                                              |
| **File System Artifacts** |                                                       |                                                                             |
| /etc/passwd            | `/etc/passwd`                                         | User account information (username, UID, GID, home directory, shell).       |
| /etc/shadow            | `/etc/shadow` (requires root privileges)              | Contains hashed passwords for local users and password policy information.  |
| /etc/group             | `/etc/group`                                          | Group information and members.                                              |
| /etc/sudoers           | `/etc/sudoers` (and `/etc/sudoers.d/`)                | Defines which users/groups can run commands as root or other users.       |
| /proc filesystem       | `/proc/` (virtual filesystem)                         | Information about running processes, kernel parameters, system hardware.    |
| /proc/[PID]/cmdline    | `/proc/[PID]/cmdline`                                 | Command line arguments of a running process.                                |
| /proc/[PID]/exe        | `/proc/[PID]/exe` (symlink)                           | Symlink to the executable file of a running process.                        |
| /proc/[PID]/maps       | `/proc/[PID]/maps`                                    | Memory mappings for a process.                                              |
| /proc/[PID]/fd/        | `/proc/[PID]/fd/`                                     | Directory containing file descriptors opened by a process.                  |
| /root                  | `/root/`                                              | Root user's home directory. Often contains attacker tools or scripts.       |
| /home/[username]       | `/home/[username]/`                                   | User home directories.                                                      |
| User Shell History     | `~/.bash_history`, `~/.zsh_history`, `~/.ash_history`, etc. | Commands executed by users in their respective shells.                    |
| /tmp, /var/tmp         | `/tmp/`, `/var/tmp/`                                  | Temporary file storage, often used by malware for staging or execution.   |
| Deleted Files (via lsof) | `lsof +L1` or `lsof /path/to/filesystem \| grep deleted` | Identify files that are deleted but still held open by a process.         |
| Crontab files          | `/etc/crontab`, `/etc/cron.d/`, `/var/spool/cron/crontabs/` | Files defining scheduled tasks for users and system.                      |
| SSH Authorized Keys    | `~/.ssh/authorized_keys`                              | Public keys authorized for passwordless SSH login to the user's account.    |
| SSH Known Hosts        | `~/.ssh/known_hosts`, `/etc/ssh/ssh_known_hosts`      | Hosts the user/system has previously connected to via SSH.                  |
| Network Configuration  | `/etc/network/interfaces`, `/etc/sysconfig/network-scripts/` (varies by distro) | Network interface configuration files.                                      |
| /dev                   | `/dev/`                                               | Device files, including disk devices (`/dev/sda`, `/dev/nvme0n1`, etc.).    |
| **System Configuration** |                                                       |                                                                             |
| /etc/hostname          | `/etc/hostname` or `/etc/sysconfig/network`           | System's hostname.                                                          |
| /etc/hosts             | `/etc/hosts`                                          | Local DNS resolution file, can be modified for C2 or phishing.            |
| /etc/resolv.conf       | `/etc/resolv.conf`                                    | DNS resolver configuration.                                                 |
| /etc/fstab             | `/etc/fstab`                                          | File system table, defines how disk partitions are mounted.                 |
| /etc/modules           | `/etc/modules`, `/etc/modules-load.d/`                | Kernel modules to load at boot.                                             |
| /etc/sysctl.conf       | `/etc/sysctl.conf`, `/etc/sysctl.d/`                  | Kernel parameter configuration.                                             |
| Service Configuration  | `/etc/systemd/system/`, `/etc/init.d/`                | Service (daemon) configuration files.                                       |
| **Memory Artifacts** |                                                       |                                                                             |
| RAM Dump               | Acquired using tools like `LiME`, `fmem`, `avml`      | A complete snapshot of the system's volatile memory (RAM).                  |
| Swap Space             | Configured swap partition or swap file (e.g., `/swapfile`) | Can contain data swapped out from RAM.                                    |
| **User Activity** |                                                       |                                                                             |
| `who` command output   | `who`                                                 | Shows who is currently logged on.                                           |
| `w` command output     | `w`                                                   | Shows who is logged on and what they are doing.                             |
| `ps` command output    | `ps aux`, `ps -ef`                                    | Lists currently running processes.                                          |
| `netstat` / `ss` output| `netstat -tulnp`, `ss -tulnp`                         | Lists network connections, listening ports, and associated processes.       |
| `find` command usage   | (Investigator initiated)                              | Used to search for files based on various criteria (name, time, size, etc.).|
| `grep` command usage   | (Investigator initiated)                              | Used to search for patterns within files.                                   |

**Note:**
* Paths can vary significantly between Linux distributions (e.g., Debian-based vs. Red Hat-based).
* Many commands require root privileges for full access to artifacts (e.g., `journalctl`, accessing `/etc/shadow`, `/proc/[PID]/`).
* The `/proc` filesystem is dynamic and reflects the current state of the system.
* Always ensure you have the legal authority and follow proper procedures when conducting investigations.
* This list is not exhaustive but covers many critical artifacts for Linux forensics.


