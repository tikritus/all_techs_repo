# Important Cybersecurity Log Event Codes

This document provides a list of important log event codes that cybersecurity analysts can refer to during investigations. The focus is primarily on Windows Security Event IDs and Sysmon Event IDs.

## Windows Security Event IDs

These events are typically found in the Windows Security Log (`%SystemRoot%\System32\winevt\Logs\Security.evtx`).

| Event ID | Event Name / Description                                      | Relevance to Investigation                                                                                                                               |
|----------|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Account Logon & Kerberos** |                                               |                                                                                                                                                          |
| 4624     | An account was successfully logged on.                        | Successful user authentications (local or domain). Look for logon type, source IP, and account name.                                                     |
| 4625     | An account failed to log on.                                  | Failed login attempts. Multiple failures can indicate brute-force or password spraying attacks. Note the Sub Status Code for reason.                     |
| 4634     | An account was logged off.                                    | User logoff activity.                                                                                                                                    |
| 4648     | A logon was attempted using explicit credentials.             | "Run as" activity. Often used by attackers for privilege escalation or lateral movement with alternate credentials.                                      |
| 4768     | A Kerberos authentication ticket (TGT) was requested.         | Kerberos TGT requests. Anomalies can indicate Kerberoasting.                                                                                             |
| 4769     | A Kerberos service ticket was requested.                      | Kerberos service ticket (TGS) requests. Anomalies (e.g., for unusual services or by unusual users) can indicate lateral movement or Kerberoasting.      |
| 4771     | Kerberos pre-authentication failed.                           | Often seen during password spraying attacks against Kerberos.                                                                                            |
| 4776     | The domain controller attempted to validate the credentials for an account (NTLM). | NTLM authentication attempts. High volumes from unexpected sources can indicate NTLM relay or brute-force attacks.                               |
| **Account Management** |                                               |                                                                                                                                                          |
| 4720     | A user account was created.                                   | Creation of new user accounts. Monitor for unauthorized account creation.                                                                                |
| 4722     | A user account was enabled.                                   | Previously disabled accounts being enabled.                                                                                                              |
| 4723     | An attempt was made to change an account's password.          | Password change attempts.                                                                                                                                |
| 4724     | An attempt was made to reset an account's password.           | Password reset attempts.                                                                                                                                 |
| 4725     | A user account was disabled.                                  | Disabling of user accounts.                                                                                                                              |
| 4726     | A user account was deleted.                                   | Deletion of user accounts.                                                                                                                               |
| 4728     | A member was added to a security-enabled global group.        | User added to a global group (e.g., Domain Admins). Critical for tracking privilege escalation.                                                          |
| 4732     | A member was added to a security-enabled local group.         | User added to a local group (e.g., Administrators). Critical for tracking privilege escalation on local machines.                                        |
| 4738     | A user account was changed.                                   | Changes to user account properties (e.g., UAC flags, SID history).                                                                                       |
| 4740     | A user account was locked out.                                | Account lockout due to too many failed login attempts.                                                                                                   |
| 4781     | The name of an account was changed.                           | Renaming of user accounts, can be used to hide activity.                                                                                                 |
| **Process Tracking** |                                               |                                                                                                                                                          |
| 4688     | A new process has been created.                               | Process creation events. Requires audit policy enablement. Look for suspicious process names, parent processes, and command lines.                     |
| 4689     | A process has exited.                                         | Process termination events.                                                                                                                              |
| **Object Access & Policy Change** |                                       |                                                                                                                                                          |
| 4656     | A handle to an object was requested.                          | Attempt to access an object (file, registry key). Requires SACL configuration.                                                                           |
| 4658     | The handle to an object was closed.                           | Closing an object handle.                                                                                                                                |
| 4660     | An object was deleted.                                        | Deletion of an object. Requires SACL configuration.                                                                                                      |
| 4663     | An attempt was made to access an object.                      | Successful access to an object (e.g., file read/write). Requires SACL configuration.                                                                     |
| 4670     | Permissions on an object were changed.                        | Changes to object permissions.                                                                                                                           |
| 4703     | A token right was adjusted.                                   | Adjusting token privileges, e.g., SeDebugPrivilege.                                                                                                      |
| 4719     | System audit policy was changed.                              | Changes to the system audit policy. Attackers may try to disable logging.                                                                                |
| 4985     | The state of a transaction has changed.                       | Related to file system transactions, can be relevant for NTFS transaction attacks.                                                                       |
| **System Events** |                                                   |                                                                                                                                                          |
| 1102     | The audit log was cleared.                                    | Indicates that the Security log (or other event logs) was cleared. Highly suspicious, often an attempt to cover tracks.                                  |
| 4616     | The system time was changed.                                  | System time changes. Attackers may alter time to disrupt logging or bypass time-based security controls.                                                 |
| 4697     | A service was installed in the system.                        | New service installation. Common persistence mechanism for malware.                                                                                      |
| 4698     | A scheduled task was created.                                 | New scheduled task creation. Common persistence mechanism.                                                                                               |
| 4699     | A scheduled task was deleted.                                 | Deletion of a scheduled task.                                                                                                                            |
| 4700     | A scheduled task was enabled.                                 | Enabling a scheduled task.                                                                                                                               |
| 4701     | A scheduled task was disabled.                                | Disabling a scheduled task.                                                                                                                              |
| 4702     | A scheduled task was updated.                                 | Modification of a scheduled task.                                                                                                                        |
| **Other Important Windows Events** |                                    |                                                                                                                                                          |
| 5140     | A network share object was accessed.                          | Access to shared files/folders. Requires "Audit File Share" policy.                                                                                      |
| 5145     | A network share object was checked to see whether client can be granted desired access. | Detailed network share access check, including relative target name and access requested.                                                                |
| 5156     | The Windows Filtering Platform has permitted a connection.    | Outbound network connection allowed by Windows Firewall.                                                                                                 |
| 5158     | The Windows Filtering Platform has permitted a bind to a local port. | Process binding to a local port.                                                                                                                       |

## Sysmon Event IDs

Sysmon (System Monitor) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to monitor and log system activity to the Windows event log. It provides detailed information about process creations, network connections, and changes to the file system.

| Event ID | Event Name / Description                                      | Relevance to Investigation                                                                                                                               |
|----------|---------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1        | Process creation.                                             | Detailed process creation information including command line, hashes, parent process, and user. Essential for tracking malware execution.              |
| 2        | A process changed a file creation time.                       | Timestomping detection. Attackers may change file timestamps to hide activity.                                                                           |
| 3        | Network connection.                                           | Logs TCP/UDP outbound network connections, including source/destination IP and port, process, and user.                                                  |
| 4        | Sysmon service state changed.                                 | Indicates Sysmon service start/stop.                                                                                                                     |
| 5        | Process terminated.                                           | Logs process termination.                                                                                                                                |
| 6        | Driver loaded.                                                | Logs loading of drivers, including hashes and signatures. Useful for detecting rootkits or malicious drivers.                                          |
| 7        | Image loaded.                                                 | Logs DLL loading by processes, including hashes and signatures. Useful for detecting DLL injection or malicious DLLs.                                  |
| 8        | CreateRemoteThread.                                           | Detects `CreateRemoteThread` API calls, often used by malware for code injection into other processes.                                                   |
| 9        | RawAccessRead.                                                | Detects processes reading directly from disk using `\\.\` notation, often used by data theft tools or disk wipers.                                     |
| 10       | ProcessAccess.                                                | Detects when a process opens another process, often a precursor to memory dumping or injection.                                                        |
| 11       | FileCreate.                                                   | Logs file creation events. Useful for tracking malware droppers or data staging.                                                                         |
| 12       | RegistryEvent (Object create and delete).                     | Logs registry key and value creation and deletion.                                                                                                       |
| 13       | RegistryEvent (Value Set).                                    | Logs registry value modifications. Crucial for detecting persistence mechanisms or configuration changes.                                                |
| 14       | RegistryEvent (Key and Value Rename).                         | Logs registry key and value rename operations.                                                                                                           |
| 15       | FileCreateStreamHash.                                         | Logs creation of alternate data streams (ADS) and hashes their content. ADS is often used by malware to hide files.                                      |
| 17       | PipeEvent (Pipe Created).                                     | Logs named pipe creation events. Named pipes are used for inter-process communication and can be used by malware for C2 or lateral movement.             |
| 18       | PipeEvent (Pipe Connected).                                   | Logs named pipe connection events.                                                                                                                       |
| 19       | WmiEvent (WmiEventFilter activity detected).                  | Logs WMI filter creation. WMI is often used for persistence and lateral movement.                                                                        |
| 20       | WmiEvent (WmiEventConsumer activity detected).                | Logs WMI consumer creation.                                                                                                                              |
| 21       | WmiEvent (WmiEventConsumerToFilter activity detected).        | Logs WMI consumer to filter binding.                                                                                                                     |
| 22       | DNSEvent (DNS query).                                         | Logs DNS queries made by processes. Essential for identifying C2 communication or connections to malicious domains.                                      |
| 23       | FileDelete (File Delete archived).                            | Logs file deletion and archives the deleted file if configured.                                                                                          |
| 24       | ClipboardChange (New content in the clipboard).               | Logs content copied to the clipboard (text only).                                                                                                        |
| 25       | ProcessTampering (Process image change).                      | Detects process hollowing or Herpaderping techniques.                                                                                                    |
| 255      | Error.                                                        | Sysmon error event.                                                                                                                                      |

**Note:**
* The availability and detail of Windows Event Logs depend on the audit policies configured on the system or domain.
* Sysmon requires installation and configuration (often with a custom XML configuration file) to generate these events.
* This list is not exhaustive but covers many of the most critical event IDs for security monitoring and incident response.

Always correlate events from multiple sources and consider the context of your specific environment.
