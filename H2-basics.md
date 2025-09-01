# Linux Task Report
| **Author**      | MR. Whoops    |
| --------------- | ------------- |
| **Date**        | 2025-09-01    |
| **Environment** | Debian Trixie |


# Command Line Basics Revisited

> Summary of Karvinen (2020)
x)
### Navigating and Viewing Files
| Command         | Description                          |                          |
| --------------- | ------------------------------------ | ------------------------ |
| `pwd`           | Prints the current working directory |                          |
| `ls`            | Lists files in the current directory |                          |
| `cd directory/` | Changes directory                    |                          |
| `cd ..`         | Moves up one dir                     |                          |


### File manipulation
| Command              | Description                           |
| -------------------- | ------------------------------------  |
| `nano filename`      | Text editor, save with CTRL-X         |
| `mkdir newfolder`    | Creates a new directory               |
| `mv oldname newname` | Moves or renames a file or directory  |
| `cp -r source dest`  | Copies directories recursively        |
| `rm filename`        | Removes a file                        |
| `rm -r folder`       | Removes a folder with all its holdings|

### emote access and file transfer
| Command                          | Description                    |
| -------------------------------- | ------------------------------ |
| `ssh user@server`                | Opens a secure remote shell    |
| `exit`                           | Exits remote shell             |
| `scp -r folder user@server:path` | Copies folder to remote server |

### Most useful of the all
| Command                          | Description                     |
| -------------------------------- | ------------------------------- |
| `man command`                    | Shows manual page               |

### Package Management
| Command                        | Description                      |
| ------------------------------ | -------------------------------- |
| `sudo apt-get update`          | Updates package lists            |
| `apt-cache search keyword`     | Searches for packages            |
| `sudo apt-get install package` | Installs a package               |
| `dpkg --listfiles package`     | Lists files installed by package |
| `sudo apt-get purge package`   | Removes package and config files |

(karvinen, 2020)



#

## a) Installing the Micro Editor

**Date and Time:** 2025-09-01 10:00

**Task:** Installed the micro editor using the apt package manager.

**Commands and Actions:**
```bash
sudo apt-get update
sudo apt-get install micro
micro
```

b) Installing and Testing Three Command-Line Programs



Chosen Programs:

nmap — network scanning tool

John the Ripper — password cracking tool

whois

Installation Command (all three at once):
```bash
sudo apt-get install nmap john whois -y
```
Testing:

### nmap 
Command:
```bash
nmap -v localhost
```

<img width="597" height="443" alt="Test with nmap" src="https://github.com/user-attachments/assets/0168f058-241a-4851-896c-f7419e64a7ca" />


### John the Ripper 
Downloaded rockyou.txt wordlist
```bash
wget https://gitlab.com/kalilinux/packages/wordlists/-/raw/kali/master/rockyou.txt.gz
```
### Unziped it
```bash
gunzip rockyou.txt.gz
```
### Run simple test with John 
```bash
john --wordlist=rockyou.txt --rules --stdout | head -10
```

### Wanted to test simple password crack with John
### Used mkpasswd (from whois package) to create the hash:
```bash
mkpasswd --method=sha-512 mrwhoop > whoop_hash.txt
```

### Then run John to crack the hash:
sudo john --wordlist=rockyou.txt whoop_hash.txt
sudo john --show whoop_hash.txt

<img width="785" height="375" alt="image" src="https://github.com/user-attachments/assets/d2a6396e-8725-4182-924e-8cccbccc0408" />


# Task is to explored the important directories
```bash
sudo -s
cd /bin #Essential user binaries
ls /bin
```
<img width="1001" height="361" alt="image" src="https://github.com/user-attachments/assets/a8ed8c8b-6c42-4c01-8e66-a06d9fdcf958" />
```bash
clear
cd /etc #Config filesin human readable format
ls -lha
```
<img width="649" height="625" alt="image" src="https://github.com/user-attachments/assets/f28ea81b-3780-4f42-9b86-053b5bf9ea5d" />

```bash
clear
cd /home #User home directory
ls 
```

# The Friendly M – Using grep

## I ran three useful grep command examples. 
### 1. 
How I find words inside file.
```bash
grep "whoops" rockyou.txt
```

### 2.
I could find the number of times string that matches the given word 
```bash
grep -c "whoops" rockyou.txt
```
<img width="561" height="485" alt="image" src="https://github.com/user-attachments/assets/45d87792-098d-40c2-a46c-e691db729cfc" />

### 3.
Search for error in all files under the /var/log directory
```bash
grep -r "error" /var/log
```

# How to use pipe
```bash
ps aux | grep ssh
```
Output:
<img width="1131" height="113" alt="image" src="https://github.com/user-attachments/assets/13f26840-9bee-4a50-8116-129bce711e9a" />

# System Overview

### What system do I run?

- **System:**  
  Standard PC (i440FX + PIIX, 1996) — This is typically a sign of emulation.  
  *(as stated by: [QEMU i386 PC](https://www.qemu.org/docs/master/system/i386/pc.html))*

- **Bus:**  
  Motherboard

- **Processor (/0/400):**  
  Intel(R) Core(TM) i5-9600K CPU running at 3.70 GHz

- **Memory (/0/1000):**  
  16 GiB of system RAM

### Bridges

Several bridge devices connect different buses (such as PCI buses) and enable communication between hardware components.

### Display (/0/100/2 /dev/fb0)

Virtual graphics card



### Network (/0/100/3 and /0/100/3/0 ens3)

Virtio-based network device and Ethernet interface **ens3**


### USB & Input Devices (/0/100/4)

USB 3.0 virtual HID devices, including virtual tablet input


### Storage (/0/100/6 and /0/100/6/0)

Virtual hard drive with 100 GB total capacity, divided into several partitions

### Generic Devices (/0/100/5, /0/100/7)

- Virtual power button  
- Two VirtualPS/2 VMware virtual mice

# Dove into my log files.
```bash
sudo journalctl -n 10
```
Interpretation and Analysis

At 16:43:48, the NetworkManager-dispatcher.service was deactivated successfully.

Between 16:51:25 and 16:51:39, the user schooldeb executed commands with sudo:

Attempted to run apt-get lshw (typo, hupsi)

Ran apt-get install

Executed lshw -short -sanitize to list hardware details

Each sudo session was properly opened and closed.

No error or warning messages are present in the logs.

Sites:
### Tero karvinen, 2020: **https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited**
### Tero karvinen: https://terokarvinen.com/linux-palvelimet/
### qemu, n.d: https://www.qemu.org/docs/master/system/i386/pc.html

