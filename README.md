# 🛡️ Lab Manual: Installing Wazuh SIEM on Ubuntu Server (VirtualBox) & Connecting via PuTTY

**Course Lab — Linux Security Operations**  
**Platform:** VirtualBox | Ubuntu Server 22.04.5 LTS | Wazuh 4.12.0 | PuTTY 0.83

---

## 📋 Table of Contents

1. [Prerequisites & Overview](#1-prerequisites--overview)
2. [Step 1 — Download Ubuntu Server ISO](#2-step-1--download-ubuntu-server-iso)
3. [Step 2 — Create a Virtual Machine in VirtualBox](#3-step-2--create-a-virtual-machine-in-virtualbox)
4. [Step 3 — Configure Virtual Hard Disk](#4-step-3--configure-virtual-hard-disk)
5. [Step 4 — Boot & Install Ubuntu Server](#5-step-4--boot--install-ubuntu-server)
6. [Step 5 — Ubuntu Installation Wizard](#6-step-5--ubuntu-installation-wizard)
7. [Step 6 — First Login to Ubuntu Server](#7-step-6--first-login-to-ubuntu-server)
8. [Step 7 — Install Wazuh (All-in-One)](#8-step-7--install-wazuh-all-in-one)
9. [Step 8 — Find the Server IP Address](#9-step-8--find-the-server-ip-address)
10. [Step 9 — Access Wazuh Web Dashboard](#10-step-9--access-wazuh-web-dashboard)
11. [Step 10 — Download & Install PuTTY](#11-step-10--download--install-putty)
12. [Step 11 — Connect to Ubuntu via PuTTY (SSH)](#12-step-11--connect-to-ubuntu-via-putty-ssh)
13. [Step 12 — Verify All Wazuh Services](#13-step-12--verify-all-wazuh-services)
14. [Common Linux Commands Reference](#14-common-linux-commands-reference)

---

## 1. Prerequisites & Overview

### What You Will Build
This lab sets up a **complete SIEM (Security Information and Event Management)** environment using:
- **VirtualBox** — runs Ubuntu Server as a virtual machine on your Windows PC
- **Ubuntu Server 22.04.5 LTS** — the Linux server operating system
- **Wazuh 4.12** — open-source security platform (manager + indexer + dashboard)
- **PuTTY** — SSH client to connect to the server remotely from Windows

### Architecture Diagram
```
Windows PC (Host)
│
├── VirtualBox
│   └── Ubuntu Server 22.04.5 LTS  (IP: 192.168.1.20)
│       ├── Wazuh Manager
│       ├── Wazuh Indexer (OpenSearch)
│       └── Wazuh Dashboard
│
└── PuTTY (SSH Client) ──────────────────► Ubuntu Server :22
    Firefox Browser   ──────────────────► Wazuh Dashboard :443
```

### Requirements
| Component | Minimum |
|-----------|---------|
| RAM | 8 GB (4 GB for VM) |
| Disk | 50 GB free |
| CPU | 4 cores (2 for VM) |
| OS | Windows 10/11 |
| VirtualBox | 7.x |

---

## 2. Step 1 — Download Ubuntu Server ISO

Go to the official Ubuntu website and download **Ubuntu Server 22.04.5 LTS**.

> 🔗 **URL:** https://ubuntu.com/download/server

<img width="742" height="537" alt="1" src="https://github.com/user-attachments/assets/ae874b7f-c318-41a5-aaad-6adeaeb6b58f" />



### What to Download
- Select **"Ubuntu Server 22.04.5 LTS"** (or the latest 22.04.x LTS)
- Click the **Download** button (Intel or AMD 64-bit, ~2 GB)
- Save the `.iso` file to a known location (e.g., `D:\Ubuntu Server 22.04.5\ISO file\`)

> ⚠️ **Note:** Image 1 shows Ubuntu 26.04 LTS on the website, but this lab uses **22.04.5 LTS** (shown in the "Previous releases" section). Download **22.04.5 LTS** for this lab.

---

## 3. Step 2 — Create a Virtual Machine in VirtualBox

Open **VirtualBox** and create a new VM for Ubuntu Server.

### Steps:
1. Click **"New"** in VirtualBox
2. Fill in the details:
   - **Name:** `Ubuntu Server 22.04.5`
   - **Folder:** `C:\Users\user\VirtualBox VMs`
   - **ISO Image:** Browse to your downloaded `.iso` file
   - **Type:** `Linux`
   - **Subtype:** `Ubuntu`
   - **Version:** `Ubuntu (64-bit)`
3. ✅ Check **"Skip Unattended Installation"**

<img width="1156" height="880" alt="2" src="https://github.com/user-attachments/assets/a005a5e5-bdaf-4701-99be-bb7acb000dd3" />


> ✅ **Important:** Always check "Skip Unattended Installation" so you can manually configure the server settings during install.

---

## 4. Step 3 — Configure Virtual Hard Disk

Expand the **"Hard Disk"** section in the VM creation wizard.

<img width="1143" height="873" alt="3" src="https://github.com/user-attachments/assets/a1023b36-da79-47e3-b8dd-fb7195d8ef96" />



### Settings:
| Setting | Value |
|---------|-------|
| Option | Create a Virtual Hard Disk Now |
| File Location | `C:\Users\user\VirtualBox VMs\Ubuntu Server 22.04.5\Ubuntu Server 22.04.5.vdi` |
| **Size** | **25.00 GB** (minimum for Wazuh) |
| File Type | VDI (VirtualBox Disk Image) |
| Pre-allocate Full Size | ❌ (leave unchecked to save host disk space) |

<img width="1143" height="873" alt="4" src="https://github.com/user-attachments/assets/ebb3090a-18d5-4245-8df3-96a98d805ec5" />


> ⚠️ **Wazuh Warning:** Wazuh requires significant disk space. Use at least **25 GB**. If disk fills up, the indexer will crash with "No space left on device" errors.

Click **"Finish"** to create the VM.

---

## 5. Step 4 — Boot & Install Ubuntu Server
<img width="1143" height="873" alt="4" src="https://github.com/user-attachments/assets/5aa736b6-30a3-44de-a45e-07cc7df034a5" />


Start the VM. It will boot from the ISO file and show the **GNU GRUB** boot menu.


### Action:
- Select **"Try or Install Ubuntu Server"** (it should be highlighted by default)
- Press **Enter** to boot

The highlighted entry will auto-execute after 22 seconds if you do nothing.

---

## 6. Step 5 — Ubuntu Installation Wizard

Follow these screens in order:
<img width="717" height="399" alt="5" src="https://github.com/user-attachments/assets/6e17876d-745e-400e-9344-5d7fd86e42e7" />


---

### 5.1 — Language Selection

<img width="820" height="610" alt="6" src="https://github.com/user-attachments/assets/8669b1a7-ff43-4fae-a473-3d9d0f090aa7" />



- Use **UP/DOWN arrow keys** to navigate
- Select **`English`** and press **Enter**

---

### 5.2 — Installation Type

<img width="805" height="592" alt="7" src="https://github.com/user-attachments/assets/0e97aeb6-9ea0-47e4-ae33-68b52e48dbf6" />




- Select **(X) Ubuntu Server** (default, full installation)
- Leave "Search for third-party drivers" **unchecked**
- Press **Enter** on **`Done`**

---

### 5.3 — Network Configuration

<img width="810" height="615" alt="8" src="https://github.com/user-attachments/assets/c448861e-4de1-480d-87d4-5bbf711df2ff" />




The installer detects the network interface automatically:
- Interface: `enp0s3` (Ethernet)
- Type: `eth`
- IP: `192.168.1.20/24` (via DHCPv4)

> This IP address is important — you'll use it to access Wazuh and connect via SSH later.

Press **Enter** on **`Done`**.

---

### 5.4 — Proxy Configuration

<img width="811" height="624" alt="9" src="https://github.com/user-attachments/assets/db3da004-5022-4a46-a0c2-601cdc8e4ca6" />



- Leave the **Proxy address** field **blank** (no proxy needed for home/lab networks)
- Press **Enter** on **`Done`**

---

### 5.5 — Ubuntu Archive Mirror

<img width="808" height="609" alt="10" src="https://github.com/user-attachments/assets/b5d90c83-fec3-4350-b2c0-39b4a79a2c4f" />




- Keep the default mirror: `http://archive.ubuntu.com/ubuntu/`
- Wait for the mirror test to complete (it says **"This mirror location passed tests"**)
- Press **Enter** on **`Done`**

---

### 5.6 — Storage (Guided)

<img width="808" height="600" alt="11" src="https://github.com/user-attachments/assets/e6101c6e-4dbe-415d-9fc2-18e547eed607" />


- Select **(X) Use an entire disk**
- The disk shown: `VBOX_HARDDISK_VBd6a17a9e-489257ff  local disk  23.000G`
- Leave LVM and encryption **unchecked** for this lab
- Press **Enter** on **`Done`**

---

### 5.7 — Storage Summary Confirmation

<img width="810" height="603" alt="12" src="https://github.com/user-attachments/assets/72219d18-dc62-4d85-8267-b021e9bd9e24" />




Review the partition layout:
| Mount Point | Size | Type | Device Type |
|-------------|------|------|-------------|
| `/` | 22.997G | new ext4 | new partition of local disk |

A 1.000M BIOS grub spacer partition is also created automatically.

- Press **Enter** on **`Done`**
- Confirm the destructive action when prompted (click **Continue**)

---

### 5.8 — Profile Setup, SSH, and Snaps

Continue through the remaining screens:

| Screen | Action |
|--------|--------|
| **Profile Setup** | Enter your name, server name (`ubuntuserver22045`), username (`abeg`), and password |
| **Ubuntu Pro** | Skip (press Done/Continue) |
| **SSH Setup** | ✅ **Enable "Install OpenSSH server"** — required for PuTTY access |
| **Featured Snaps** | Skip (press Done) |

> ✅ **Critical:** Make sure to enable OpenSSH server! Without it, PuTTY cannot connect.

The installation will now proceed automatically (takes 5–15 minutes). When done, select **"Reboot Now"**.

---

## 7. Step 6 — First Login to Ubuntu Server

After reboot, the server shows the login prompt. Enter your credentials.

<img width="693" height="482" alt="13" src="https://github.com/user-attachments/assets/c927b4f1-7574-440a-9e35-87bdc1dc30c5" />


```
Ubuntu 22.04.5 LTS ubuntuserver22045 tty1
ubuntuserver22045 login: nahima
Password: ••••••••
```

You will see a system summary:

| Info | Value |
|------|-------|
| System load | 0.56 |
| Usage of / | 32.5% of 22.47GB |
| Memory usage | 5% |
| IPv4 address | 192.168.1.20 |

<img width="365" height="219" alt="14" src="https://github.com/user-attachments/assets/88fa29cb-1c13-4770-9923-8c934caa762b" />


> Notice **"71 updates can be applied immediately"** — run `sudo apt update && sudo apt upgrade -y` to update the system first (optional but recommended).

---

## 8. Step 7 — Install Wazuh (All-in-One)

Wazuh provides a single-command installer that sets up the Manager, Indexer, and Dashboard together.

### Step 7.1 — Download the Wazuh Install Script

```bash
sudo curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
```

<img width="769" height="120" alt="15" src="https://github.com/user-attachments/assets/0b2f41c6-61f3-4c61-a549-3b178d0971a4" />



- `curl -sO` — downloads the file silently and saves it with the original filename
- The script will be saved as `wazuh-install.sh` in your current directory

### Step 7.2 — Run the Installer

```bash
sudo bash wazuh-install.sh -a
```

<img width="829" height="79" alt="16" src="https://github.com/user-attachments/assets/4aff1ae5-8359-437e-a230-396893017be1" />



The `-a` flag means **all-in-one** — installs everything on a single server:
- Wazuh Indexer (OpenSearch)
- Wazuh Manager
- Wazuh Dashboard

> ⏳ **This takes 20–40 minutes.** The installer checks hardware requirements, downloads packages, and configures all services.

You will see progress logs like:
```
16/05/2026 19:09:00 INFO: Starting Wazuh installation assistant. Wazuh version: 4.12.0
16/05/2026 19:09:00 INFO: Verbose logging redirected to /var/log/wazuh-install.log
16/05/2026 19:09:07 INFO: Verifying that your system meets the recommended minimum hardware requirements.
```

### Step 7.3 — Installation Complete — Save Your Credentials!
<img width="814" height="106" alt="17" src="https://github.com/user-attachments/assets/27620845-7f0a-46dc-9acf-ad763d9ee9db" />



When finished, the installer displays a **summary with login credentials**:

```
16/05/2026 19:42:47 INFO: --- Summary ---
16/05/2026 19:42:47 INFO: You can access the web interface https://<wazuh-dashboard-ip>:443
    User: admin
    Password: tYTZbB1J35o?*GX64xc4+kg3XsjFH5Fg
16/05/2026 19:42:47 INFO: Installation finished.
```

> 🔴 **IMPORTANT: Save this password immediately!** Copy it to a text file. You cannot recover it without resetting.

---

## 9. Step 8 — Find the Server IP Address

Run the `ip a` command to confirm the server's IP address:

```bash
ip a
```

<img width="814" height="106" alt="17" src="https://github.com/user-attachments/assets/53cc4fe9-c012-486e-925d-951db322a3a6" />


Look for the `enp0s3` interface:
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.20/24  brd 192.168.1.255  scope global dynamic enp0s3
```

> **Your Wazuh Dashboard URL:** `https://192.168.1.20`  
> **Your SSH address:** `192.168.1.20` on port `22`

---

## 10. Step 9 — Access Wazuh Web Dashboard

Open **Firefox** (or any browser) on your Windows host machine and navigate to:

```
https://192.168.1.20
```

### 10.1 — Security Warning (Expected)

<img width="814" height="240" alt="18" src="https://github.com/user-attachments/assets/c64f1a3c-f458-4d7a-aaef-cb2948847888" />


Firefox shows a security warning because Wazuh uses a **self-signed certificate**. This is normal for lab environments.

**To proceed:**
1. Click **"Advanced..."**
2. Click **"Accept the Risk and Continue"**

The error code `SEC_ERROR_UNKNOWN_ISSUER` is expected — it just means the certificate wasn't issued by a trusted public CA.

### 10.2 — Wazuh Login Page

<img width="1600" height="732" alt="19" src="https://github.com/user-attachments/assets/923d9689-e199-41dc-994e-0d5c94e77536" />


Enter your credentials from Step 7.3:
- **Username:** `admin`
- **Password:** *(the password shown during installation)*

Click **"Log in"**.

---

### 10.3 — Wazuh Dashboard Overview

<img width="1600" height="678" alt="20" src="https://github.com/user-attachments/assets/6a605599-ca2d-4cc8-a5f2-c36f5595f449" />


You are now inside the Wazuh security platform! The dashboard shows:

**Agents Summary**
- No agents registered yet (deploy agents to monitor endpoints)

**Last 24 Hours Alerts**
| Severity | Count | Rule Level |
|----------|-------|------------|
| Critical | 0 | 15 or higher |
| High | 0 | 12 to 14 |
| Medium | 117 | 7 to 11 |
| Low | 111 | 0 to 6 |

**Available Modules:**
- Endpoint Security: Configuration Assessment, Malware Detection, File Integrity Monitoring
- Threat Intelligence: Threat Hunting, Vulnerability Detection, MITRE ATT&CK
- Security Operations: PCI DSS, GDPR, HIPAA, NIST 800-53
- Cloud Security: Docker, AWS, Google Cloud, GitHub

---

## 11. Step 10 — Download & Install PuTTY

PuTTY is a free SSH client for Windows that lets you connect to your Ubuntu server remotely.

> 🔗 **URL:** https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html

<img width="1600" height="676" alt="21" src="https://github.com/user-attachments/assets/9348c1ff-2461-4fe2-8a81-f558032eea79" />


### Download:
- Under **"Package files" → MSI ('Windows Installer')**
- Select **64-bit x86:** `putty-64bit-0.83-installer.msi`
- Run the installer and follow the setup wizard (Next → Next → Install)

---

## 12. Step 11 — Connect to Ubuntu via PuTTY (SSH)

Open PuTTY from the Start Menu.

### PuTTY Configuration

<img width="1600" height="899" alt="22" src="https://github.com/user-attachments/assets/268ff840-22d7-4fe6-9777-802224df3a7e" />


Fill in the settings:

| Field | Value |
|-------|-------|
| **Host Name (or IP address)** | `192.168.1.20` |
| **Port** | `22` |
| **Connection type** | `SSH` ✅ |

**Optional:** Save this session for future use:
1. Type a name in **"Saved Sessions"** (e.g., `Lab Work`)
2. Click **"Save"**

Click **"Open"** to connect.

### First Connection — Accept Host Key

On first connect, PuTTY shows a security alert about the server's host key. Click **"Accept"** to trust this server.

### SSH Login

<img width="721" height="712" alt="23" src="https://github.com/user-attachments/assets/19964df4-a4e3-4c11-be63-bca558be825e" />


```
login as: nahima
nahima@192.168.1.20's password: ••••••••
```

<img width="1591" height="802" alt="24" src="https://github.com/user-attachments/assets/777adbdf-1374-44bb-ac8b-aca64af3f738" />


You are now **remotely connected** to your Ubuntu server via SSH! You can run all Linux commands from your Windows machine through this terminal.

---

## 13. Step 12 — Verify All Wazuh Services

After connecting via PuTTY, verify that all three Wazuh components are running:

### Check Wazuh Manager

```bash
systemctl status wazuh-manager --no-pager
```

<img width="848" height="306" alt="25" src="https://github.com/user-attachments/assets/675225fb-0c76-4d1b-82f3-88ec03ab0794" />


**Expected output:**
```
● wazuh-manager.service - Wazuh manager
   Loaded: loaded (/lib/systemd/system/wazuh-manager.service; enabled)
   Active: active (running) since Sat 2026-05-16 19:42:35 UTC; 1h 6min ago
   Tasks: 164
  Memory: 1.7G
```

All sub-processes should be running: `wazuh-authd`, `wazuh-db`, `wazuh-execd`, `wazuh-analysisd`, `wazuh-syscheckd`, `wazuh-remoted`, `wazuh-logcollector`, `wazuh-monitord`, `wazuh-modulesd`

---

### Check Wazuh Dashboard

```bash
systemctl status wazuh-dashboard --no-pager
```

<img width="746" height="318" alt="26" src="https://github.com/user-attachments/assets/e748d0ff-808a-457e-8692-f9365f93e243" />


**Expected output:**
```
● wazuh-dashboard.service - wazuh-dashboard
   Active: active (running) since Sat 2026-05-16 19:42:37 UTC; 1h 8min ago
  Memory: 206.2M
```

---

### Check Wazuh Indexer

```bash
systemctl status wazuh-indexer --no-pager
```

<img width="1600" height="405" alt="27" src="https://github.com/user-attachments/assets/e404eb9c-99e2-436c-9790-8bdd1168ed57" />


**Expected output:**
```
● wazuh-indexer.service - wazuh-indexer
   Active: active (running) since Sat 2026-05-16 19:23:57 UTC; 1h 25min ago
  Memory: 1.1G
```

> ⚠️ **Troubleshooting:** If the indexer shows errors like `"No space left on device"`, your virtual disk is full. You may need to increase the VDI size or clean up space with `sudo journalctl --vacuum-size=100M`.

---

### All Services Running ✅

| Service | Status | Port |
|---------|--------|------|
| wazuh-manager | ✅ active (running) | 1514, 1515, 55000 |
| wazuh-indexer | ✅ active (running) | 9200 |
| wazuh-dashboard | ✅ active (running) | 443 |

---

## 14. Common Linux Commands Reference

These are essential Linux commands used throughout this lab and in general server administration:

### Navigation & File Management

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print working directory (where you are) | `pwd` → `/home/abeg` |
| `ls` | List files and folders | `ls -la` (detailed list) |
| `cd` | Change directory | `cd /var/ossec/` |
| `mkdir` | Create a directory | `mkdir myfolder` |
| `rm` | Remove file | `rm file.txt` |
| `rm -r` | Remove directory recursively | `rm -r myfolder` |
| `cp` | Copy file | `cp file.txt /tmp/` |
| `mv` | Move or rename file | `mv old.txt new.txt` |
| `cat` | Display file contents | `cat /etc/os-release` |
| `less` | View file with scroll | `less /var/log/syslog` |
| `nano` | Simple text editor | `nano myfile.txt` |

### System Information

| Command | Description | Example |
|---------|-------------|---------|
| `uname -a` | Show kernel and OS info | `uname -a` |
| `hostname` | Show server name | `hostname` |
| `uptime` | How long system has been running | `uptime` |
| `top` | Live process monitor | `top` (press `q` to quit) |
| `htop` | Enhanced process monitor | `htop` |
| `df -h` | Disk usage (human readable) | `df -h` |
| `free -h` | RAM usage | `free -h` |
| `lscpu` | CPU information | `lscpu` |

### Networking

| Command | Description | Example |
|---------|-------------|---------|
| `ip a` | Show all IP addresses | `ip a` |
| `ip r` | Show routing table | `ip r` |
| `ping` | Test connectivity | `ping 8.8.8.8` |
| `ss -tulnp` | Show open ports | `ss -tulnp` |
| `curl` | Download/test URLs | `curl -sO https://example.com/file` |
| `wget` | Download files | `wget https://example.com/file` |
| `nslookup` | DNS lookup | `nslookup google.com` |

### Package Management (Ubuntu/Debian)

| Command | Description | Example |
|---------|-------------|---------|
| `sudo apt update` | Refresh package list | `sudo apt update` |
| `sudo apt upgrade -y` | Upgrade all packages | `sudo apt upgrade -y` |
| `sudo apt install` | Install a package | `sudo apt install curl -y` |
| `sudo apt remove` | Remove a package | `sudo apt remove curl` |
| `apt list --upgradable` | List upgradable packages | `apt list --upgradable` |
| `dpkg -l` | List installed packages | `dpkg -l \| grep wazuh` |

### Service Management (systemd)

| Command | Description | Example |
|---------|-------------|---------|
| `systemctl status` | Check service status | `systemctl status wazuh-manager` |
| `systemctl start` | Start a service | `sudo systemctl start wazuh-manager` |
| `systemctl stop` | Stop a service | `sudo systemctl stop wazuh-manager` |
| `systemctl restart` | Restart a service | `sudo systemctl restart wazuh-dashboard` |
| `systemctl enable` | Enable service on boot | `sudo systemctl enable wazuh-manager` |
| `systemctl disable` | Disable service on boot | `sudo systemctl disable wazuh-manager` |
| `journalctl -u` | View service logs | `journalctl -u wazuh-manager -f` |

### User & Permissions

| Command | Description | Example |
|---------|-------------|---------|
| `sudo` | Run command as root | `sudo apt update` |
| `whoami` | Show current user | `whoami` |
| `id` | Show user ID and groups | `id abeg` |
| `passwd` | Change password | `passwd` |
| `chmod` | Change file permissions | `chmod 755 script.sh` |
| `chown` | Change file owner | `sudo chown root:root file.txt` |
| `su` | Switch user | `su - root` |

### Searching & Text Processing

| Command | Description | Example |
|---------|-------------|---------|
| `grep` | Search for text in files | `grep "error" /var/log/syslog` |
| `find` | Find files by name | `find / -name "*.sh"` |
| `tail` | View end of file (live logs) | `tail -f /var/log/syslog` |
| `head` | View beginning of file | `head -20 /var/log/auth.log` |
| `wc -l` | Count lines | `cat file.txt \| wc -l` |
| `sort` | Sort output | `ls \| sort` |
| `\|` (pipe) | Send output to next command | `ls -la \| grep ".sh"` |

### Wazuh-Specific Commands

| Command | Description |
|---------|-------------|
| `sudo systemctl status wazuh-manager` | Check Wazuh manager status |
| `sudo systemctl status wazuh-indexer` | Check indexer (OpenSearch) status |
| `sudo systemctl status wazuh-dashboard` | Check dashboard status |
| `sudo /var/ossec/bin/agent_control -l` | List all registered agents |
| `sudo tail -f /var/ossec/logs/ossec.log` | Live Wazuh manager logs |
| `sudo cat /var/log/wazuh-install.log` | View full installation log |

---

## 🎯 Lab Summary

You have successfully:

1. ✅ **Downloaded** Ubuntu Server 22.04.5 LTS ISO
2. ✅ **Created** a VirtualBox VM with 25 GB disk
3. ✅ **Installed** Ubuntu Server with OpenSSH enabled
4. ✅ **Installed** Wazuh 4.12 (all-in-one: Manager + Indexer + Dashboard)
5. ✅ **Accessed** the Wazuh web dashboard at `https://192.168.1.20`
6. ✅ **Connected** to the server remotely via PuTTY (SSH)
7. ✅ **Verified** all Wazuh services are running

---

*Lab Manual prepared for Linux Security Operations course.*  
*Tools: Ubuntu 22.04.5 LTS · Wazuh 4.12.0 · VirtualBox · PuTTY 0.83*
