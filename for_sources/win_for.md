# Windows Cybersecurity Investigation Artifacts

This document provides a list of important files, log locations, forensic artifacts, and registry paths relevant to cybersecurity investigations on Windows operating systems.

| Artifact Type          | Path/Location                                       | Description                                                                 |
|------------------------|-------------------------------------------------------|-----------------------------------------------------------------------------|
| **Event Logs** |                                                       |                                                                             |
| Security Log           | `%SystemRoot%\System32\winevt\Logs\Security.evtx`     | Records security-related events like logon attempts, resource access, etc.  |
| System Log             | `%SystemRoot%\System32\winevt\Logs\System.evtx`       | Records system-level events, driver issues, hardware failures.              |
| Application Log        | `%SystemRoot%\System32\winevt\Logs\Application.evtx`  | Records events logged by applications.                                      |
| PowerShell Logs        | `%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-PowerShell%4Operational.evtx` | Records PowerShell command execution.                                         |
| PowerShell Script Block Logging | Group Policy or Registry (see Registry section) | Logs the actual content of PowerShell scripts executed.                     |
| TerminalServices/RemoteConnectionManager | `%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-TerminalServices-LocalSessionManager%4Operational.evtx` | Records RDP session activity (logons, logoffs, disconnections).             |
| Task Scheduler Logs    | `%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-TaskScheduler%4Operational.evtx` | Records task creation, modification, deletion, and execution.               |
| Windows Defender Logs  | `%SystemRoot%\System32\winevt\Logs\Microsoft-Windows-Windows Defender%4Operational.evtx` | Records Windows Defender AV detections and actions.                         |
| **File System Artifacts** |                                                       |                                                                             |
| Prefetch Files         | `%SystemRoot%\Prefetch\`                             | Stores information about applications that have been run on the system.     |
| Shimcache (AppCompatCache) | Registry (see Registry section)                     | Stores information about application compatibility, including executable paths. |
| Amcache.hve            | `%SystemRoot%\AppCompat\Programs\Amcache.hve`         | Contains information about recently run applications and their SHA1 hashes. |
| $LogFile               | `C:\$LogFile` (requires raw disk access)              | NTFS file system journal, records metadata changes to files and directories.  |
| $MFT                   | `C:\$MFT` (requires raw disk access)                  | Master File Table, contains information about all files and directories on an NTFS volume. |
| Recycle Bin            | `C:\$Recycle.Bin\<User SID>\`                         | Contains deleted files.                                                     |
| Alternate Data Streams (ADS) | File specific (e.g., `file.txt:hidden_stream`)      | Allows data to be stored in hidden streams attached to files.               |
| Volume Shadow Copies   | System managed, accessible via tools like `vssadmin`  | Snapshots of disk volumes, can contain older versions of files.             |
| Jump Lists             | `%APPDATA%\Microsoft\Windows\Recent\AutomaticDestinations\` & `%APPDATA%\Microsoft\Windows\Recent\CustomDestinations\` | Stores recently accessed files and applications by users.                   |
| LNK Files (Shortcuts)  | Various locations (Desktop, Recent Items, Startup)    | Contain metadata about the target file/application, including original path and timestamps. |
| Pagefile.sys           | `C:\pagefile.sys`                                     | System paging file, stores data swapped out from RAM.                       |
| Hiberfil.sys           | `C:\hiberfil.sys`                                     | Stores system memory content when the system hibernates.                    |
| **Registry Artifacts** |                                                       |                                                                             |
| SAM Hive               | `%SystemRoot%\System32\config\SAM`                    | Stores local user account information and password hashes.                  |
| SYSTEM Hive            | `%SystemRoot%\System32\config\SYSTEM`                 | Contains system configuration information, services, drivers, boot settings. |
| SOFTWARE Hive          | `%SystemRoot%\System32\config\SOFTWARE`               | Contains software installation information and settings.                    |
| NTUSER.DAT             | `C:\Users\<Username>\NTUSER.DAT`                      | Stores user-specific registry settings.                                     |
| UsrClass.dat           | `C:\Users\<Username>\AppData\Local\Microsoft\Windows\UsrClass.dat` | Stores user-specific COM object and shell extension information.        |
| Run/RunOnce Keys       | `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` <br> `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run` <br> `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` <br> `HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce` (and Wow6432Node equivalents) | Programs listed here automatically run at startup.                          |
| Services               | `HKLM\SYSTEM\CurrentControlSet\Services`              | Configuration information for system services.                              |
| UserAssist             | `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist\{GUID}\Count` | Encoded information about GUI programs executed by the user.                |
| Shimcache (AppCompatCache) Registry | `HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache` or `AppCompatibility` | Stores information about application compatibility.                         |
| Network Interfaces     | `HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\` | Information about network interfaces and their configurations.              |
| USBStor                | `HKLM\SYSTEM\CurrentControlSet\Enum\USBSTOR`          | Information about USB devices that have been connected to the system.       |
| PowerShell Script Block Logging (Registry) | `HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging` <br> `HKCU\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging` | Enable/Disable PowerShell Script Block Logging. `EnableScriptBlockLogging=1` |
| **Memory Artifacts** |                                                       |                                                                             |
| RAM Dump               | Acquired using tools like `dumpit.exe`, `FTK Imager`  | A complete snapshot of the system's volatile memory (RAM).                  |
| **Browser Artifacts** |                                                       |                                                                             |
| History                | Browser specific (e.g., Chrome: `C:\Users\<User>\AppData\Local\Google\Chrome\User Data\Default\History`) | Records visited websites.                                                   |
| Cache                  | Browser specific (e.g., Edge: `C:\Users\<User>\AppData\Local\Microsoft\Edge\User Data\Default\Cache`) | Stores temporary internet files.                                            |
| Cookies                | Browser specific                                      | Small files stored by websites to remember user preferences or session info. |
| Downloads              | Browser specific and `C:\Users\<User>\Downloads`       | Records downloaded files.                                                   |
| **Other Artifacts** |                                                       |                                                                             |
| SRUM/ESE Database      | `%SystemRoot%\System32\sru\SRUDB.dat`                 | System Resource Usage Monitor - tracks process execution, network usage.    |
| Windows Timeline       | `C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\<random_folder>\ActivitiesCache.db` | SQLite database storing user activity history.                              |
| Scheduled Tasks Files  | `%SystemRoot%\System32\Tasks\`                        | XML files defining scheduled tasks.                                         |

**Note:**
* Paths starting with `%SystemRoot%` usually refer to `C:\Windows`.
* `%APPDATA%` usually refers to `C:\Users\<Username>\AppData\Roaming`.
* `HKLM` refers to `HKEY_LOCAL_MACHINE` in the registry.
* `HKCU` refers to `HKEY_CURRENT_USER` in the registry.
* Accessing some artifacts (like `$MFT`, `$LogFile`, raw disk) requires specialized forensic tools and administrative privileges.
* The exact paths and availability of artifacts can vary slightly between Windows versions.
* Always ensure you have the legal authority and follow proper procedures when conducting investigations.

This list is not exhaustive but covers many of the critical artifacts commonly examined during Windows forensic investigations.
