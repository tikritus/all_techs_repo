# Important Linux Log Indicators for Cybersecurity

This document provides a list of important log files and key indicators (keywords, message patterns) that cybersecurity analysts can refer to during investigations on Linux systems. Unlike Windows Event IDs, Linux logs are often text-based, and analysis relies on pattern matching and keyword searching.

## Common Linux Log Files & Indicators

| Log Source / File(s)             | Keywords / Message Patterns / Tools                                  | Relevance to Investigation                                                                                                                                                              |
|------------------------------------|----------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Authentication & Authorization** |                                                                      |                                                                                                                                                                                         |
| `/var/log/auth.log` (Debian/Ubuntu) <br> `/var/log/secure` (RHEL/CentOS/Fedora) | `Accepted publickey for`, `Accepted password for`, `Failed password for`, `invalid user`, `authentication failure`, `session opened for user`, `session closed for user`, `sudo:`, `su:`, `new user`, `new group`, `userdel`, `groupdel` | Tracks successful and failed logins (SSH, local, su, sudo), user/group management, and authentication methods. Critical for identifying unauthorized access, brute-force attempts, and privilege escalation. |
| `last` command (reads `/var/log/wtmp`) | (Output shows user, tty, source IP, login time, duration)            | Shows historical user login sessions. Useful for identifying unusual login times or sources.                                                                                              |
| `lastb` command (reads `/var/log/btmp`)| (Output shows failed login attempts)                                 | Shows failed login attempts, helping to identify brute-force attacks or attempts to guess credentials.                                                                                    |
| `journalctl -u sshd` (if using systemd) | (ssh-specific logs from the journal)                                 | Filters journald logs specifically for SSH daemon activity.                                                                                                                               |
| **System & Service Activity** |                                                                      |                                                                                                                                                                                         |
| `/var/log/syslog` (Debian/Ubuntu) <br> `/var/log/messages` (RHEL/CentOS/Fedora) | `error`, `failed`, `warning`, `critical`, service names (e.g., `sshd`, `cron`, `named`), kernel messages (`kernel:`) | General system messages, service start/stop/errors, hardware issues, and kernel-level events. Can indicate system instability, service misconfigurations, or malware activity.         |
| `journalctl` (if using systemd)    | `_SYSTEMD_UNIT=service_name.service`, `PRIORITY=crit` (or `err`, `warning`) | Centralized logging system. Allows filtering by service, priority, time, etc. Provides a comprehensive view of system and service activity.                                             |
| `/var/log/kern.log`                | `kernel:`, driver messages, hardware errors                            | Kernel-specific messages. Useful for diagnosing hardware issues, driver failures, or kernel panics that might be exploited or caused by malware.                                        |
| **Process & Cron Activity** |                                                                      |                                                                                                                                                                                         |
| `/var/log/cron` (or in syslog/messages) | `CRON`, `CMD`, `run-parts`, specific script names                      | Logs execution of scheduled tasks (cron jobs). Attackers often use cron for persistence. Look for unusual scripts or execution times.                                                       |
| `auditd` logs (`/var/log/audit/audit.log`) | `type=SYSCALL`, `type=EXECVE`, `type=PATH`, `key=` (if using audit rules) | Detailed system call auditing, including process execution (`execve`), file access, and network connections. Requires `auditd` to be configured with appropriate rules. Very powerful for deep forensics. |
| Shell history files (`~/.bash_history`, `~/.zsh_history`, etc.) | (User-executed commands)                                             | Records commands executed by users. Can reveal attacker actions, tools used, and reconnaissance activities. Note: Can be tampered with.                                                   |
| **Network Activity** |                                                                      |                                                                                                                                                                                         |
| Firewall logs (e.g., `iptables`, `ufw`, `firewalld` - often log to syslog/messages or dedicated files) | `UFW BLOCK`, `UFW ALLOW`, `iptables:`, `REJECT`, `DROP`, `ACCEPT`, `SRC=`, `DST=`, `PROTO=` | Logs network connections that were allowed or denied by the firewall. Helps identify C2 communication, scanning activity, or policy violations.                                           |
| Web server logs (e.g., `/var/log/apache2/access.log`, `/var/log/nginx/access.log`) | HTTP status codes (4xx, 5xx), suspicious User-Agents, SQL injection patterns (`' OR '1'='1`), LFI/RFI patterns (`../../`), shell commands in URLs | Records requests to web servers. Can reveal web attacks, reconnaissance, exploitation attempts, and C2 communication over HTTP/S.                                                             |
| DNS client logs (less common by default, often requires specific configuration or tools like `dnsmasq` logging) | (Queried domains, resolver IP)                                       | Can reveal DNS queries to malicious domains or C2 infrastructure if client-side DNS logging is enabled.                                                                                   |
| **Package Management Logs** |                                                                      |                                                                                                                                                                                         |
| `/var/log/dpkg.log` (Debian/Ubuntu) | `install`, `remove`, `upgrade`, package names                          | Logs package installation, removal, and upgrades using `dpkg`. Can show installation of malicious or unauthorized software.                                                               |
| `/var/log/yum.log` (RHEL/CentOS < 8) <br> `/var/log/dnf.log` (RHEL/CentOS >= 8, Fedora) | `Installed:`, `Erased:`, `Updated:`, package names                     | Logs package management activities using `yum` or `dnf`. Can reveal installation of malicious RPMs or unauthorized software.                                                                |
| **Application Specific Logs** |                                                                      |                                                                                                                                                                                         |
| Various (e.g., `/var/log/mysql/error.log`, `/var/log/gitlab/`, `/opt/someapp/logs/`) | Application-specific error messages, access patterns, security events    | Logs generated by specific applications. Content and relevance vary greatly. Important for investigating compromises of those applications.                                                   |

**Key Tools for Linux Log Analysis:**
* `grep`: For searching text patterns within files.
* `awk`: For text processing and extracting specific fields.
* `sed`: For stream editing and text transformation.
* `sort`, `uniq`: For sorting and counting unique lines.
* `less`, `more`: For paging through log files.
* `journalctl`: For querying the systemd journal.
* `ausearch`: For querying `auditd` logs.
* `logwatch`, `logcheck`: Automated log analysis tools.

**Note:**
* Log file locations and names can vary slightly between Linux distributions and configurations.
* Log rotation policies might mean older logs are compressed (e.g., `.gz`) or moved.
* Sufficient logging levels must be configured for services to capture relevant details.
* Attackers may attempt to delete or tamper with log files. Look for gaps or signs of modification.

This table provides a starting point for identifying key areas to investigate within Linux logs. Effective analysis often involves correlating information from multiple log sources.
