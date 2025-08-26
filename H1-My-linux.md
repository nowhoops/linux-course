# My Own Linux

## x) Read and Summarize

### Top Rules of Reporting

- Report anything and everything that you did and what happened.
- Make it repeatable.
- Be precise.
- Cite your sources.

### What Not to Do

- **NO FABRICATION**
- **NO PLAGIARISM**
- **DON’T SAY YOU DID STUFF YOU DIDN’T**

---

## a) Install Linux on a Virtual Machine

> Create a new virtual machine for the report, even if you have installed Linux before.

### Preparing

- **Date**: 2025-08-25  
- **Host System**: TrueNAS server with additional compute resources  
- **VM Location**: `/mnt/POOL01/Application/VM`  
- **Access Controls**: Set necessary permissions for users on the dataset

I chose TrueNAS as the VM host because I already use it for NAS purposes, and its VM features have worked fine in the past.  
My chosen Linux distribution was **Debian 13 “Trixie”**, the latest release.

---

### Setting Up the VM

I set up a new dataset:

- **Path**: `/mnt/POOL01/Application/VM`
- I gave necessary access controls to users.

#### Virtual Machine Configuration

| Setting              | Value                          |
|----------------------|--------------------------------|
| Operating System     | Linux                          |
| VM Name              | Schooldeb                      |
| Description          | School Debian                  |
| System Clock         | UTC                            |
| Boot Method          | UEFI                           |
| Shutdown Timeout     | 90 seconds                     |
| Start on Boot        | Enabled                        |
| Enable Display       | Enabled                        |
| Bind                 | `<IP address>`                 |
| Password             | `<password>`                   |
| Virtual CPUs         | 1                              |
| CPU Cores            | 2                              |
| CPU Threads          | 2                              |
| CPU Mode             | Host Passthrough               |
| Memory Size          | 8 GB                           |
| Minimum Memory Size  | 4 GB                           |

---

## Installing the OS

- Started the VM and verified that it boots on startup.
- Connected via the TrueNAS web console (VNC).
- Selected **Graphical Install**

### Installation Settings

- **Language / Keyboard**: English / Finland
- **Hostname**: Schooldeb
- **Domain Name**: Left empty (using own DNS server)
- **Root Password**: Left empty

#### New User:

- **Full Name**: `<user>`
- **Username**: `<user>`
- **Password**: `<password>`

- **Time Zone**: Eastern (time synchronized via NTP server)

### Partitioning

- **Method**: Guided – use entire disk
- **Disk**: Virtual disk 1 (vda) – 53.7 GB Virtio Block Device
- **Scheme**: All files in one partition `/` (Finnish partitioning)
- Wrote changes to disk
- Do not scan for extra installation media
- **Mirror country**: Finland
- **Mirror**: deb.debian.org

### Software Selection

- Debian desktop environment
- **Xfce**
- SSH server
- Standard system utilities

All installation steps completed successfully without errors.

---

## Post-Install Configuration

- **Hostname**: Schooldeb
- **Domain**: Left empty (I use my own DNS)
- **Root password**: Left empty
- **Time Zone**: Eastern (NTP takes care of correct time)
- **Partitioning**: One partition `/` using Finnish partitioning
- After reboot, removed installation media from TrueNAS

### Basic Tests

```bash
sudo apt update
ping 1.1.1.1
ip a
sudo apt install x11-apps
xeyes &

 ![Kuvaus](upload/xeyes.png)

Karvinen, 2023
Basic Firewall Settings

sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw enable

k) Optional Bonus: My Favorite Program on Linux

I used xeyes, a fun graphical tool:

xeyes &

Lähteet

Tero Karvinen, 2023. Luettu:23.08 Luettavissa:
https://github.com/terokarvinen/dreamhugmonkey#adding-images-to-markdown.
