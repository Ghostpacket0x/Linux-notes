# Linux Practice Notes

A beginner-friendly guide to the Linux commands I practised on **Kali Linux in a VirtualBox VM**. Every section starts with a plain-English explanation, then lists the commands, what they do, and an example you can try.

> Only run these commands on systems you own or have permission to test.

## Contents

- [Before you start](#before-you-start)
1. [Files and navigation](#1-files-and-navigation)
2. [Viewing and editing text](#2-viewing-and-editing-text)
3. [Processes and system information](#3-processes-and-system-information)
4. [Disk usage](#4-disk-usage)
5. [Dates and calendars](#5-dates-and-calendars)
6. [Networking](#6-networking)
7. [Users and groups](#7-users-and-groups)
8. [Packages](#8-packages)
9. [Services and logs](#9-services-and-logs)
10. [Terminal multiplexing with tmux](#10-terminal-multiplexing-with-tmux)
11. [Serving files over HTTP](#11-serving-files-over-http)
12. [Troubleshooting log](#12-troubleshooting-log)
13. [Key lessons](#key-lessons)

---

## Before you start

**Key words**

| Word | What it means |
|---|---|
| Terminal | The window where you type commands instead of clicking |
| Command | An instruction you type and run by pressing Enter |
| Directory | Another word for a folder |
| Root | The all-powerful administrator account on Linux. Also the name of the top folder `/` |
| `sudo` | Runs one command with administrator rights. Linux asks for your password |
| PID | Process ID, the number Linux gives every running program |
| Package | A piece of software you install with a package manager |
| Service | A program that runs in the background and waits for requests, like SSH |
| Port | A numbered door on a computer that a service listens on, like 22 for SSH |

**How to read the examples**

- Anything in `<angle brackets>` is a placeholder. Replace it with your own value. For example, `<name>` could become `alex`.
- The `|` symbol is called a pipe. It sends the output of one command into the next one.
- Linux is **case-sensitive**: `Notes.txt` and `notes.txt` are different files.
- Press the **Tab** key while typing a name and Linux will try to finish it for you.
- Press **Ctrl+C** to stop a command that is running.

**Getting help when you are stuck**

- `<command> --help` shows a short summary, for example `ls --help`.
- `man <command>` opens the full manual, for example `man ls`. Press `q` to leave it.

---

## 1. Files and navigation

Linux stores everything in folders (directories). These commands let you see where you are, move around, and create, copy, move, or delete files.

| Command | What it does | Example |
|---|---|---|
| `pwd` | Shows the folder you are in right now | `pwd` |
| `ls` | Lists the files and folders here | `ls` |
| `ls -l` | Long list: permissions, owner, size, and date | `ls -l` |
| `ls -a` | Includes hidden files (names that start with a dot) | `ls -a` |
| `ls -h` | With `-l`, shows sizes like 4K or 2M instead of raw bytes | `ls -lh` |
| `ls -lah` | The three above combined, a very common choice | `ls -lah` |
| `ls -F` | Adds a marker: `/` for folders, `*` for programs | `ls -F` |
| `cd <folder>` | Moves into a folder | `cd Documents` |
| `cd ..` | Goes up one folder | `cd ..` |
| `cd ~` | Goes to your home folder | `cd ~` |
| `cd /` | Goes to the very top of the system (the root folder) | `cd /` |
| `which <command>` | Shows where a command is stored | `which nmap` |
| `find . -name "<pattern>"` | Searches the current folder for names that match | `find . -name "file1*"` |
| `find / -type f` | Lists every file on the system (very long) | `find / -type f` |
| `tree` | Draws folders as a tree (you may need to install it) | `tree` |
| `touch <file>` | Creates an empty file | `touch notes.txt` |
| `mkdir <folder>` | Creates a folder | `mkdir labs` |
| `cp <from> <to>` | Copies a file | `cp notes.txt backup.txt` |
| `mv <from> <to>` | Moves a file, or renames it | `mv notes.txt labs/` |
| `rm <file>` | Deletes a file. **There is no recycle bin** | `rm backup.txt` |
| `rm -r <folder>` | Deletes a folder and everything inside it | `rm -r old-labs` |
| `rmdir <folder>` | Deletes an empty folder | `rmdir empty-folder` |
| `./script.sh` | Runs a script in the current folder. It needs permission first: `chmod +x script.sh` | `./script.sh` |

**Archives (bundling files together)**

An archive packs many files into one file, which is useful for backups.

| Command | What it does | Example |
|---|---|---|
| `tar -czf <name>.tar.gz <folder>` | Creates a compressed archive. `c` = create, `z` = compress, `f` = file name | `tar -czf backup.tar.gz labs/` |
| `tar -tf <archive>` | Lists what is inside without extracting | `tar -tf backup.tar.gz` |
| `tar -xzf <archive>` | Extracts an archive. `x` = extract | `tar -xzf backup.tar.gz` |
| `zip -r <name>.zip <folder>` | Creates a zip file. `-r` includes subfolders | `zip -r backup.zip labs/` |
| `unzip <file>.zip` | Extracts a zip file | `unzip backup.zip` |

## 2. Viewing and editing text

Most Linux settings and logs are plain text files. These commands let you read them, search them, and change them.

| Command | What it does | Example |
|---|---|---|
| `cat <file>` | Prints the whole file | `cat notes.txt` |
| `head <file>` | Shows the first 10 lines | `head notes.txt` |
| `tail <file>` | Shows the last 10 lines | `tail notes.txt` |
| `less <file>` | Lets you scroll through a long file. Press `q` to quit | `less notes.txt` |
| `nano <file>` | A beginner-friendly editor. `Ctrl+O` then Enter saves, `Ctrl+X` exits | `nano notes.txt` |
| `vim <file>` | A powerful editor. Press `i` to type, then `Esc` and `:wq` to save and quit, or `:q!` to quit without saving | `vim notes.txt` |
| `grep "<word>" <file>` | Shows the lines that contain a word | `grep "error" log.txt` |
| `grep -A3 "<word>" <file>` | Also shows 3 lines after each match | `grep -A3 "error" log.txt` |
| `sort <file>` | Sorts the lines alphabetically | `sort names.txt` |
| `tr 'a-z' 'A-Z'` | Swaps characters. This example makes text uppercase | `echo hello \| tr 'a-z' 'A-Z'` |
| `strings <file>` | Pulls readable text out of a program or binary file | `strings /bin/ls` |
| `file "<name>"` | Tells you what type of file something is | `file notes.txt` |
| `wc <file>` | Counts lines, words, and bytes | `wc notes.txt` |
| `wc -c <file>` | Counts bytes only | `wc -c notes.txt` |
| `wc -L <file>` | Shows the length of the longest line | `wc -L notes.txt` |
| `awk '{print $1}' <file>` | Prints the first word (column) of every line | `awk '{print $1}' notes.txt` |
| `cut -d: -f1 <file>` | Splits each line at `:` and prints field 1 | `cut -d: -f1 /etc/passwd` |
| `bc` | A calculator in the terminal | `echo "5*3" \| bc` |
| `whatis <command>` | Gives a one-line description of a command | `whatis ls` |
| `apropos <word>` | Finds commands related to a word | `apropos copy` |

## 3. Processes and system information

A **process** is a running program. These commands show what is running and what your machine is made of.

**Processes**

| Command | What it does | Example |
|---|---|---|
| `ps` | Lists processes in your current terminal | `ps` |
| `ps aux` | Lists every process from every user | `ps aux` |
| `ps auxf` | Same list, drawn as a tree of parent and child processes | `ps auxf` |
| `pstree -p` | A tree view with each PID | `pstree -p` |
| `ps -p <PID> -o <columns>` | Shows chosen details about one process | `ps -p 1 -o pid,user,cmd` |
| `ps aux \| grep bash` | Finds processes whose line contains "bash" | `ps aux \| grep bash` |
| `top` | A live view of running processes. Press `q` to quit | `top` |
| `htop` | A friendlier version of `top` (you may need to install it) | `htop` |
| `jobs` | Lists programs running in the background of this terminal | `jobs` |
| `kill <PID>` | Asks a process to stop | `kill 1234` |
| `watch -n <seconds> <command>` | Repeats a command on a timer | `watch -n 5 free -h` |

**Memory and system details**

| Command | What it does | Example |
|---|---|---|
| `free` | Shows memory (RAM) and swap usage | `free` |
| `free -m` / `-g` / `-h` | Shows it in megabytes, gigabytes, or an easy-to-read form | `free -h` |
| `uname` | Shows the operating system name | `uname` |
| `uname -a` | Shows kernel version and system details | `uname -a` |
| `id` | Shows who you are: your user ID and groups | `id` |
| `who` | Shows who is logged in | `who` |
| `w` | Shows who is logged in and what they are doing | `w` |

**Hardware**

| Command | What it does | Example |
|---|---|---|
| `lsblk` | Lists disks and partitions | `lsblk` |
| `lsusb` | Lists USB devices | `lsusb` |
| `lscpu` | Shows details about the processor | `lscpu` |
| `lspci` | Lists internal devices like the graphics and network cards | `lspci` |

## 4. Disk usage

Two questions come up often: "how big is this folder?" and "how much space is left?"

| Command | What it does | Example |
|---|---|---|
| `du` | Shows the space each folder uses, in raw blocks | `du` |
| `du -h` | The same, in an easy-to-read form | `du -h` |
| `du -sh <folder>` | A single total for one folder (`s` = summary) | `du -sh Documents` |
| `df` | Shows free space on each disk | `df` |
| `df -h` | The same, in an easy-to-read form | `df -h` |

## 5. Dates and calendars

| Command | What it does | Example |
|---|---|---|
| `date` | Shows today's date and the time | `date` |
| `cal` | Shows this month's calendar | `cal` |
| `ncal` | Shows it with the days of the week down the side | `ncal` |
| `cal <year>` | Shows a whole year | `cal 2027` |
| `cal <month> <year>` | Shows one month | `cal 1 2027` |
| `cal -m <month> <year>` | Another way to pick a month | `cal -m 5 1990` |

## 6. Networking

These commands help you see how a machine talks to others. They are the basics of troubleshooting and security checks.

| Command | What it does | Example |
|---|---|---|
| `ss -tulpn` | Lists listening ports. `t` TCP, `u` UDP, `l` listening, `p` which program, `n` show numbers | `ss -tulpn` |
| `ss -lntp` | Listening TCP ports only | `ss -lntp` |
| `netstat -a \| less` | The older tool: all connections, one page at a time | `netstat -a \| less` |
| `netstat -lt \| less` | Listening TCP ports | `netstat -lt \| less` |
| `netstat -lu` | Listening UDP ports | `netstat -lu` |
| `netstat -st` | Statistics for TCP | `netstat -st` |
| `netstat -su` | Statistics for UDP | `netstat -su` |
| `ping -c 4 <IP>` | Sends 4 test messages to check a host answers | `ping -c 4 8.8.8.8` |
| `nmap -sV <IP>` | Scans for open ports and the software behind them | `nmap -sV 127.0.0.1` |

**Notes**

- `netstat` is old. `ss` is the modern replacement and gives the same kind of information.
- `127.0.0.1` always means "this computer", so it is a safe address to practise `nmap` on.
- Only scan machines you own or have written permission to test.

## 7. Users and groups

Linux can have many users, and each one can belong to groups that control what they may access. Creating and removing users is a normal admin job.

**Creating and changing users**

| Command | What it does | Example |
|---|---|---|
| `sudo adduser <name>` | Creates a user with a home folder, and asks you questions step by step | `sudo adduser alex` |
| `sudo useradd -m -s /bin/bash <name>` | The low-level version. `-m` makes a home folder, `-s` sets the shell | `sudo useradd -m -s /bin/bash alex` |
| `sudo passwd <name>` | Sets or changes a password | `sudo passwd alex` |
| `sudo usermod -aG <group> <name>` | Adds a user to a group. `-a` means append, so existing groups are kept | `sudo usermod -aG sudo alex` |
| `sudo usermod -L <name>` | Locks an account | `sudo usermod -L alex` |
| `sudo usermod -U <name>` | Unlocks an account | `sudo usermod -U alex` |

**Deleting users**

| Command | What it does | Example |
|---|---|---|
| `sudo deluser <name>` | Deletes a user but keeps their home folder | `sudo deluser alex` |
| `sudo userdel <name>` | The low-level version of the same job | `sudo userdel alex` |

**Groups and permissions**

| Command | What it does | Example |
|---|---|---|
| `sudo addgroup <group>` | Creates a group | `sudo addgroup students` |
| `sudo groupadd <group>` | The low-level way to create a group | `sudo groupadd students` |
| `sudo delgroup <group>` | Deletes a group | `sudo delgroup students` |
| `sudo visudo` | Safely edits the file that decides who may use `sudo` | `sudo visudo` |

**Checking accounts**

| Command | What it does | Example |
|---|---|---|
| `id <name>` | Shows a user's ID and groups | `id alex` |
| `getent passwd <name>` | Checks whether an account exists | `getent passwd alex` |
| `cut -d: -f1,3 /etc/passwd` | Lists every account name with its ID number | `cut -d: -f1,3 /etc/passwd` |
| `sudo cat /etc/passwd` | Shows the list of accounts | `sudo cat /etc/passwd` |
| `sudo cat /etc/group` | Shows the list of groups | `sudo cat /etc/group` |
| `sudo cat /etc/shadow` | Shows password hashes. **Never share this output** | `sudo cat /etc/shadow` |

## 8. Packages

A package manager is like an app store for the terminal. It downloads, installs, updates, and removes software for you. Kali uses **apt**.

**The apt commands**

| Command | What it does | Example |
|---|---|---|
| `sudo apt update` | Refreshes the list of available software. It does not install anything | `sudo apt update` |
| `sudo apt update --fix-missing` | Refreshes the list and retries files that failed | `sudo apt update --fix-missing` |
| `sudo apt upgrade` | Installs newer versions of what you already have | `sudo apt upgrade` |
| `sudo apt full-upgrade` | Like upgrade, but may add or remove packages when needed | `sudo apt full-upgrade` |
| `sudo apt install <pkg>` | Installs software | `sudo apt install tree` |
| `sudo apt remove <pkg>` | Removes software but keeps its settings | `sudo apt remove tree` |
| `sudo apt purge <pkg>` | Removes software and its settings | `sudo apt purge tree` |
| `-y` (added to the end) | Answers "yes" to the confirmation prompt automatically | `sudo apt install tree -y` |
| `sudo apt autoremove` | Removes software that was only installed to support other software | `sudo apt autoremove` |
| `sudo apt clean` | Deletes downloaded installer files to free space | `sudo apt clean` |
| `apt search <word>` | Searches for packages | `apt search nmap` |
| `apt show <pkg>` | Shows details about a package | `apt show nmap` |
| `apt list --installed` | Lists what is installed | `apt list --installed` |
| `apt list --upgradable` | Lists what has an update waiting | `apt list --upgradable` |

**dpkg (the tool underneath apt)**

| Command | What it does | Example |
|---|---|---|
| `dpkg -l` | Lists installed packages | `dpkg -l` |
| `dpkg -s <pkg>` | Shows the status of one package | `dpkg -s nmap` |
| `dpkg -L <pkg>` | Lists the files a package installed | `dpkg -L nmap` |
| `sudo dpkg -i <file>.deb` | Installs a downloaded `.deb` file | `sudo dpkg -i app.deb` |
| `sudo dpkg -r <pkg>` | Removes a package | `sudo dpkg -r app` |
| `sudo dpkg --configure -a` | Finishes installs that were interrupted | `sudo dpkg --configure -a` |

**Other tools**

| Command | What it is for |
|---|---|
| `sudo aptitude full-upgrade` | Another upgrade tool with its own way of solving conflicts |
| `snap` | Installs apps packaged as snaps |
| `pip` | Installs Python libraries |
| `gem` | Installs Ruby libraries |
| `git` | Downloads and tracks source code |

## 9. Services and logs

A **service** is a program that runs quietly in the background and waits for requests, such as SSH (remote login). Linux manages services with `systemctl`, and records what happens in logs you can read with `journalctl`.

| Command | What it does | Example |
|---|---|---|
| `systemctl status ssh` | Shows whether the SSH service is running | `systemctl status ssh` |
| `sudo systemctl start ssh` | Starts the service now | `sudo systemctl start ssh` |
| `sudo systemctl enable ssh` | Makes it start automatically every time the machine boots | `sudo systemctl enable ssh` |
| `sudo systemctl disable ssh` | Stops it starting automatically at boot | `sudo systemctl disable ssh` |
| `journalctl` | Shows the system logs | `journalctl` |
| `journalctl -u <service>` | Shows the logs for one service only | `journalctl -u ssh.service` |

> Only run an SSH service on machines you own, and turn it off when you don't need it. A running service is a door that others can try to open.

## 10. Terminal multiplexing with tmux

`tmux` lets you split one terminal window into several panes and keep sessions running even if you close the window. This is useful for long jobs such as system upgrades.

Start it with `tmux`. After that, every shortcut begins with `Ctrl+b`: press `Ctrl+b`, let go, then press the next key.

| Keys or command | What it does |
|---|---|
| `tmux` | Starts a session |
| `Ctrl+b` then `%` | Splits the window into left and right panes |
| `Ctrl+b` then `"` | Splits the window into top and bottom panes |
| `Ctrl+b` then arrow keys, or `o` | Moves between panes |
| `Ctrl+b` then `x` | Closes the current pane |
| `Ctrl+b` then `d` | Detaches: leaves the session running in the background |
| `tmux attach -t <name>` | Reconnects to a session. Use `tmux ls` to see session names |
| `Ctrl+b` then `:` and `set -g status off` | Hides the status bar at the bottom |

## 11. Serving files over HTTP

You can turn any folder into a tiny website with one command. This is handy for sharing files between your VM and your own computer, or for testing.

| Command | What it does | Example |
|---|---|---|
| `npx http-server -p 8080` | Serves the current folder using Node.js (needs npm installed) | `npx http-server -p 8080` |
| `php -S 127.0.0.1:8080` | Serves the current folder using PHP's built-in server | `php -S 127.0.0.1:8080` |

- Open `http://localhost:8080` in a browser to see the files.
- `-p 8080` picks the port number.
- `127.0.0.1` means only your own machine can connect.
- Stop either server with `Ctrl+C`.
- Only serve folders you are happy for others to see.

## 12. Troubleshooting log

Real problems I hit while practising, why they happened, and how I fixed them. Fixing errors is the best way to learn.

| Problem | Why it happens | Fix |
|---|---|---|
| `Failed to fetch ... 404 Not Found` during apt | The package list is out of date, so apt looks for files that no longer exist | `sudo apt clean`, then `sudo apt update`, then `sudo apt full-upgrade -y` |
| `Could not open lock file ... Permission denied` | apt needs administrator rights and was run without them | Add `sudo`: `sudo apt update` |
| `npx: command not found` | npm is not installed | `sudo apt install npm` |
| `Error opening configuration file` for `.ovpn` | The file is not in the folder given | Find it with `find ~ -name "1.ovpn"`, then use the full path |
| `cannot open display:` for a GUI app | The app was started from a terminal that has no desktop connection | Launch it from the desktop menu |
| `unknown cmd` in tmux | A normal shell command was typed at the tmux prompt | Use `Ctrl+b` then `:` for tmux commands only |
| aptitude proposed removing 32 packages | A dependency conflict: it wanted to remove software to make the upgrade work | Rejected the plan and used `apt full-upgrade` instead |
| `su -` authentication failure | Root has no password set on Kali | Use `sudo` instead |
| Leftover `/home/<name>` folder | The account was deleted without its home folder | Backed it up, checked the contents, then removed it |
| `deluser` backup archive was empty | The account did not exist | Confirmed with `id`, `getent`, and `ls /home` |
