# macOS Cybersecurity Investigation Artifacts

This document provides a list of important files, log locations, and forensic artifacts relevant to cybersecurity investigations on macOS operating systems.

| Artifact Type                 | Path/Location                                                                 | Description                                                                                                |
|-------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Log Files** |                                                                               |                                                                                                            |
| Unified Logs (logarchive)     | `/var/db/diagnostics`, `/var/db/uuidtext`, `log show` command                 | Centralized logging system in macOS (10.12+). Contains system and application messages.                     |
| System Log (pre-10.12)        | `/var/log/system.log`                                                         | Legacy system log. Still may contain some information or be relevant for older systems.                  |
| Secure Log                    | `/var/log/secure.log` (may be part of system.log or unified logs)             | Authentication-related events, sudo usage, SSH logins.                                                     |
| Install Log                   | `/var/log/install.log`                                                        | Information about software installations and updates.                                                      |
| Audit Logs                    | `/var/audit/` (requires configuration: `audit_control` file)                  | Detailed system call auditing, file access, security events (if enabled and configured).                   |
| CUPS (Printing) Logs          | `/var/log/cups/`                                                              | Logs related to printing activity.                                                                         |
| Firewall Log                  | `/var/log/appfirewall.log` (ALF) or check `pfctl` for PF logging              | Application Layer Firewall logs or Packet Filter (PF) logs if configured.                                  |
| Bash/Zsh History              | `~/.bash_history`, `~/.zsh_history`                                           | Command line history for users.                                                                            |
| **File System Artifacts** |                                                                               |                                                                                                            |
| User Accounts & Groups        | `/var/db/dslocal/nodes/Default/users/` & `/var/db/dslocal/nodes/Default/groups/` | User and group account information (plist files). Use `dscl . -list /Users` or `dscacheutil`.              |
| Sudoers File                  | `/etc/sudoers` (and `/etc/sudoers.d/`)                                        | Defines which users/groups can run commands with root privileges.                                          |
| Bash/Zsh Profile & RC files | `~/.bash_profile`, `~/.bashrc`, `~/.zprofile`, `~/.zshrc`, `/etc/profile`, `/etc/bashrc`, `/etc/zshrc` | Shell configuration files, can be used for persistence or environment modification.                      |
| LaunchAgents & LaunchDaemons  | `~/Library/LaunchAgents/` <br> `/Library/LaunchAgents/` <br> `/Library/LaunchDaemons/` <br> `/System/Library/LaunchAgents/` <br> `/System/Library/LaunchDaemons/` | Property list (.plist) files that define services and applications to launch at login or boot. Common persistence mechanism. |
| StartupItems (Legacy)         | `/Library/StartupItems/`, `/System/Library/StartupItems/`                       | Older mechanism for launching items at boot.                                                               |
| Login Items                   | `~/Library/Application Support/com.apple.backgroundtaskmanagementagent/backgrounditems.btm` and via System Settings/Preferences | Applications set to launch at user login.                                                                  |
| Kernel Extensions (Kexts)     | `/Library/Extensions/`, `/System/Library/Extensions/`                         | Kernel modules that extend OS functionality. Can be used by rootkits.                                      |
| System Extensions             | App-specific locations, managed by the OS. `systemextensionsctl list`         | Modern replacement for Kexts, run in userspace.                                                            |
| Quarantine Events             | `sqlite3 ~/Library/Preferences/com.apple.LaunchServices.QuarantineEventsV2`   | Database tracking files downloaded from the internet, including source URL and timestamp.                  |
| Spotlight Metadata            | `.Spotlight-V100` directory at the root of each volume (e.g., `/.Spotlight-V100`) | Indexes file metadata and content for quick searching. Can reveal existence of deleted files.            |
| FSEvents                      | `/.fseventsd/` (at the root of each volume)                                   | Logs file system events (creations, modifications, deletions). Requires specialized parsers.               |
| Trash                         | `~/.Trash/`, `/.Trashes/<UID>/` (on external/network volumes)                 | Deleted files.                                                                                             |
| Recently Used Items           | `~/Library/Application Support/com.apple.sharedfilelist/`                     | Contains lists of recently opened documents and applications (SFL/SFL2 files).                             |
| QuickLook Thumbnails          | `~/Library/Caches/com.apple.quicklook.ThumbnailsAgent/` and `/var/folders/`   | Cached thumbnails of files viewed with QuickLook.                                                          |
| Saved Application State       | `~/Library/Saved Application State/`                                          | Stores the state of applications when they are closed, allowing them to resume.                            |
| Mobile Backups (iOS/iPadOS)   | `~/Library/Application Support/MobileSync/Backup/`                            | Backups of connected iOS/iPadOS devices.                                                                   |
| **System Configuration** |                                                                               |                                                                                                            |
| Hostname                      | `scutil --get HostName`, `/etc/hostconfig` (older)                            | System's hostname.                                                                                         |
| Hosts File                    | `/etc/hosts`                                                                  | Local DNS resolution file.                                                                                 |
| Network Configuration         | `~/Library/Preferences/SystemConfiguration/preferences.plist` <br> `networksetup` command | Network interface settings, Wi-Fi history, VPN configurations.                                           |
| Gatekeeper & XProtect         | System managed, `spctl` command                                               | Security features that control application execution and check for known malware. XProtect Yara rules at `/System/Library/CoreServices/XProtect.bundle/Contents/Resources/XProtect.yara`. |
| **User Activity & Plists** |                                                                               |                                                                                                            |
| Finder Preferences            | `~/Library/Preferences/com.apple.finder.plist`                                | User's Finder settings, recently accessed folders.                                                         |
| Dock Preferences              | `~/Library/Preferences/com.apple.dock.plist`                                  | Applications pinned to the Dock, recent applications.                                                      |
| System Preferences (Settings) | Various `.plist` files in `~/Library/Preferences/`                            | User-configured system settings.                                                                           |
| Keychain Files                | `~/Library/Keychains/login.keychain-db`, `/Library/Keychains/System.keychain` | Stores passwords, certificates, and other sensitive data. Requires user's password to unlock.              |
| Stickies Database             | `~/Library/StickiesDatabase`                                                  | Content of Stickies notes.                                                                                 |
| Notes Database                | `~/Library/Group Containers/group.com.apple.notes/NoteStore.sqlite`           | Content of Apple Notes.                                                                                    |
| **Browser Artifacts (Safari)**|                                                                               |                                                                                                            |
| History                       | `~/Library/Safari/History.db`                                                 | Browsing history (SQLite database).                                                                        |
| Downloads                     | `~/Library/Safari/Downloads.plist`                                            | List of downloaded files.                                                                                  |
| Last Session                  | `~/Library/Safari/LastSession.plist`                                          | Tabs and windows open during the last browsing session.                                                    |
| Cookies                       | `~/Library/Cookies/Cookies.binarycookies`                                     | Safari cookies.                                                                                            |
| Top Sites                     | `~/Library/Safari/TopSites.plist`                                             | Frequently visited sites.                                                                                  |
| Local Storage / Web Data      | `~/Library/Safari/LocalStorage/`, `~/Library/Safari/Databases/`                | HTML5 local storage and web databases.                                                                     |
| **Other Browser Artifacts** | (Chrome, Firefox paths are similar to Windows/Linux but under `~/Library/Application Support/`) |                                                                                                            |
| Chrome History                | `~/Library/Application Support/Google/Chrome/Default/History`                 |                                                                                                            |
| Firefox History               | `~/Library/Application Support/Firefox/Profiles/<profile>/places.sqlite`      |                                                                                                            |
| **Memory Artifacts** |                                                                               |                                                                                                            |
| RAM Dump                      | Acquired using tools like `osxpmem`                                           | A snapshot of the system's volatile memory.                                                                |
| Sleepimage                    | `/private/var/vm/sleepimage`                                                  | Stores RAM contents when the Mac sleeps (hibernates).                                                      |
| Swap Files                    | `/private/var/vm/swapfile*`                                                   | Virtual memory swap files.                                                                                 |
| **APFS Snapshots** |                                                                               |                                                                                                            |
| Local Time Machine Snapshots  | `tmutil listlocalsnapshots /`                                                 | APFS file system can create local snapshots, useful for recovering previous file versions.                 |

**Note:**
* `~` refers to the current user's home directory (e.g., `/Users/username/`).
* Accessing many of these artifacts requires administrative (root) privileges.
* Paths and artifact availability can change between macOS versions.
* SIP (System Integrity Protection) may restrict access to certain system locations even for root.
* Always ensure you have legal authority and follow proper procedures.


