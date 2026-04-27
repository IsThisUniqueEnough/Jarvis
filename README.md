# Jarvis AIO Toolkit

This is an all-in-one script I built while I was first learning the game. I have since moved on to other projects, and there are definitely much better and more optimized tools out there. However, I'm making this available in the hopes that it helps newer players understand the game's mechanics, how to explore networks, and how to structure a larger script.

The goal of this project was to pack as many useful utilities as possible into a single script to avoid managing multiple files. 

## Key Features

* **Single-File Build:** The entire toolkit, including payloads and the file manager, compiles from a single `.src` file.
* **Custom Encryption (JarvisCrypt):** To keep your databases secure, the script uses a custom encryption scheme. It encrypts cache files like your recorded root passwords, reverse shell targets, and bounce IP caches using a configurable `MASTER_KEY`. Mostly for fun, it's not good encryption.
* **ExplorOS:** A built-in terminal file manager. Instead of dealing with standard shell commands, you can drop into ExplorOS to natively navigate directories, upload/download payloads, move files, and visually map directory trees.
* **Object Memory & Focus:** Jarvis tracks your movements. Every shell, computer, or file you interact with is saved to the script's memory. You can list your captured targets and switch your active "Focus" to any of them instantly, saving you from having to re-exploit machines or juggle terminal tabs.

## Command Reference

The toolkit divides commands into two main categories based on operational security. To use any command that interacts with a target, you must first have that target set as your active "Focus".

### Ghost Protocol (Stealth / Local)
These commands are generally safe to use and do not leave aggressive system logs on the target.

* `nmap [ip]` - Passive port scan. Use `-v` for verbose output (note: verbose mode will interact with the target and leave logs).
* `ls` - Opens the focused target in the ExplorOS visual file manager. **Note:** One of the best features of ExplorOS is that it only requires file-level access for basic functionality. If you manage to get a `file` object instead of a full `shell`, you can still use this command to navigate the directory structure, read files, etc, without needing execution privileges.
* `objs` - Lists all network targets currently stored in your session memory.
* `foc [id]` - Changes your active focus to a specific object ID (e.g., `foc 2`) or `foc me` to return focus to your local machine.
* `sift` - Automatically crawls the target's filesystem looking for user folders, reading configuration files and extracting passwords without executing any binaries.
* `wlog` - Attempts to corrupt and wipe `/var/system.log` on the target to clear your tracks.
* `dc [hash]` - Deciphers an MD5 hash using the local crypto engine.
* `opendb` - Decrypts and opens one of the Jarvis databases in the GUI text editor so you can edit it manually.
* `ps` - Lists all running processes on the focused target.
* `kill [pid]` - Terminates a specific process ID on the target.
* `log` - Launches the GUI LogViewer.exe directly on the target machine.
* `fe` - Launches the GUI FileExplorer.exe directly on the target machine.

### Aggro Protocol (Active Exploitation)
These commands execute code, upload files, or perform actions that will leave logs and interact heavily with the target. Use with caution or behind a proxy.

* `scan [ip] [port]` - Directly scans a port for memory vulnerabilities and attempts exploitation.
* `proxy` - Deploys a clone of the Jarvis script onto your currently focused shell. All subsequent actions will be routed through this target, effectively masking your origin IP from logs. This is essential for operational security. Run it as soon as you gain an initial foothold.
* `su [pass]` - Attempts to escalate privileges to root on the focused shell. If you run it with a password (`su mypassword`), it attempts to log in. If you run it without arguments (`su`), it uploads and executes an automated password cracking script via RCE to brute-force the root account. Successful cracks are automatically saved to your database.
* `lle` - Local Library Exploit. Scans the target's `/lib` folder to find local exploits for privilege escalation. Best used after you have established a `proxy` on the network.
* `deepmap [ip]` - Uploads a payload to recursively map out nested network topologies, revealing switches and firewall rules via RCE.
* `scanlan` - Uploads and executes ScanLan.exe on the target to discover other devices on that local network.
* `rshell [opt]` - A suite of tools to manage reverse shells. Use `rshell install` to drop the service on a target, `rshell db` to view your list of infected machines, `rshell listen` to start catching connections, and `rshell queue` to view and interact with active callbacks.
* `troj` - Deploys a trojanized `ps` binary to the target. This replaces the standard process viewer, hiding your reverse shell connections from anyone checking the running tasks.
* `launch [binary]` - Executes a specific binary on the target machine. It automatically searches `/bin`, `/usr/bin`, and the user's home directory to find the file.
* `bash` - Opens a standard terminal window directly on the target.
* `ssh [ip] [port] [user] [pass]` - Manually connect to a target via SSH. The resulting shell is automatically tracked in your memory list.
* `reboot` - Sends a reboot signal to the target and safely drops the connection from your Jarvis memory to prevent the script from crashing when the host goes down.

## Experimental 

I also built a few multifunction "all-in-one" commands to automate larger attacks. **These are experimental and buggy.** I don't recommend relying on them if you are actually trying to play optimally, but the code is there if you want to see how to string multiple actions together.

* `autohack [ip]` - Attempts to fully infiltrate, escalate to root, and clean logs in one go.
* `spear [ip]` - Tries to bounce through a LAN, find user emails, and crack `Mail.txt` files for passwords.
* `rpivot [wan]` - Sweeps a LAN to cache bounce exploits and automatically capture internal devices.
* `macro [run/edit]` - Automates a predefined SSH proxy chain. Use `macro edit` to set up your connection list in a text file, and `macro run` to execute the sequence and tunnel your connection automatically.
* `nuke` - A scorched-earth script that deletes `/boot`, scrubs logs, and drops the connection, destroying the target machine.
