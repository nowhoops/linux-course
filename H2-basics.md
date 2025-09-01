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

hydra — network login cracker

Installation Command (all three at once):
```bash
sudo apt-get install nmap john whois -y
```
Testing and Observations:

nmap 
Command:
```bash
nmap -v localhost
```

<img width="597" height="443" alt="Test with nmap" src="https://github.com/user-attachments/assets/0168f058-241a-4851-896c-f7419e64a7ca" />


John the Ripper 
### Downloaded rockyou.txt wordlist
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



**https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited**

