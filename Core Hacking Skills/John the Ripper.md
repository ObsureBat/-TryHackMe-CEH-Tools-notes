# 🧠 John the Ripper (JTR) – CEH Practical Notes (Complete)

---

# 📌 1. Introduction

- Tool: **John the Ripper (Jumbo John)**
    
- Used for:
    
    - Hash cracking
        
    - Password recovery
        
    - CTF / Pentesting
        

---

## ✅ Verify Installation

```bash
john
```

✔ Output should show:

```
John the Ripper 1.9.0-jumbo
```

---

# 🧩 2. Hash Basics

---

## 🔐 What is a Hash?

- One-way function
    
- Fixed-length output
    

---

## 🔹 Generate Hash (Practice)

```bash
echo -n "polo" | md5sum
```

---

## 📊 Hash Identification (IMPORTANT)

|Length|Type|
|---|---|
|32|MD5|
|40|SHA1|
|64|SHA256|

---

## 🔍 Identify Hash Tool

```bash
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
python3 hash-id.py
```

---

## 🧠 Diagram

```
Input → Hash Function → Hash Output
```

---

# ⚙️ 3. Setup & Wordlists

---

## 📂 Default Wordlists

```bash
ls /usr/share/wordlists/
```

---

## 📥 Extract RockYou (if zipped)

```bash
gunzip /usr/share/wordlists/rockyou.txt.gz
```

---

## 📦 Download Large Wordlist (optional)

```bash
wget https://github.com/danielmiessler/SecLists/raw/master/Passwords/Common-Credentials/10-million-password-list-top-1000000.txt
```

---

# ⚔️ 4. Basic Hash Cracking

---

## 🔹 Basic Syntax

```bash
john [options] hash.txt
```

---

## 🔹 Automatic Mode

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

---

## 🔹 Format-Specific

```bash
john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

---

## 🔹 List Formats

```bash
john --list=formats | grep -i md5
```

---

## 🔹 Show Password

```bash
john --show hash.txt
```

---

## 🧠 Diagram

```
Hash → Identify → Wordlist → John → Password
```

---

# 🪟 5. Windows Hash Cracking (NTLM)

---

## 📌 Hash Source

- SAM Database
    
- NTDS.dit
    
- Tools: Mimikatz
    

---

## 🔹 Crack NTLM

```bash
john --format=nt --wordlist=/usr/share/wordlists/rockyou.txt ntlm.txt
```

---

## 🔹 Show Result

```bash
john --show ntlm.txt
```

---

# 🐧 6. Linux (/etc/shadow) Cracking

---

## 📂 Combine Files

```bash
unshadow passwd shadow > unshadowed.txt
```

---

## 🔓 Crack Hash

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt unshadowed.txt
```

---

## 🔹 Specify Format (if needed)

```bash
john --format=sha512crypt --wordlist=rockyou.txt unshadowed.txt
```

---

## 🧠 Diagram

```
passwd + shadow
      ↓
   unshadow
      ↓
unshadowed.txt
      ↓
     john
      ↓
   password
```

---

# 🎯 7. Single Crack Mode

---

## 📌 Format File

```bash
nano hash.txt
```

Add:

```
joker:7bf6d9bb82bed1302f331fc6b816aada
```

---

## 🔹 Run Single Mode

```bash
john --single --format=Raw-MD5 hash.txt
```

---

## 🔹 Show Result

```bash
john --show hash.txt
```

---

## 🧠 Diagram

```
Username → Word Mangling → Variations → Match → Password
```

---

# ⚙️ 8. Custom Rules

---

## 📂 Edit Config

```bash
nano /etc/john/john.conf
```

---

## 🔹 Add Rule

```
[List.Rules:THMRules]
cAz"[0-9][!@#]"
```

---

## 🔹 Use Rule

```bash
john --wordlist=rockyou.txt --rules=THMRules hash.txt
```

---

## 🔹 Test Rules (optional)

```bash
john --wordlist=rockyou.txt --rules hash.txt
```

---

## 🧠 Diagram

```
Wordlist → Rules → Modified Passwords → John → Crack
```

---

# 📦 9. ZIP File Cracking

---

## 🔹 Extract Hash

```bash
zip2john secure.zip > zip_hash.txt
```

---

## 🔹 Crack

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```

---

## 🔹 Show

```bash
john --show zip_hash.txt
```

---

## 🧠 Diagram

```
ZIP → zip2john → hash → john → password
```

---

# 📁 10. RAR File Cracking

---

## 🔹 Extract Hash

```bash
rar2john secure.rar > rar_hash.txt
```

---

## 🔹 Crack

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```

---

## 🔹 Show

```bash
john --show rar_hash.txt
```

---

## 🧠 Diagram

```
RAR → rar2john → hash → john → password
```

---

# 🔑 11. SSH Key Cracking

---

## 🔹 Convert Key

```bash
python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
```

---

## 🔹 Crack

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

---

## 🔹 Show

```bash
john --show id_rsa_hash.txt
```

---

## 🧠 Diagram

```
id_rsa → ssh2john → hash → john → password
```

---

# 🚀 12. Advanced Modes

---

## 🔹 Rules Mode

```bash
john --wordlist=rockyou.txt --rules hash.txt
```

---

## 🔹 Incremental Mode (Brute Force)

```bash
john --incremental hash.txt
```

---

## 🔹 Fork (Speed)

```bash
john --fork=2 hash.txt
```

---

# ⚡ 13. Quick Command Cheat Sheet

```bash
john hash.txt
john --wordlist=rockyou.txt hash.txt
john --format=Raw-MD5 hash.txt
john --single hash.txt
john --rules hash.txt
john --incremental hash.txt
john --show hash.txt
```

---

# 🎯 14. CEH Practical Strategy

---

## ✅ Step-by-Step Approach

```
1. Identify hash
2. Try auto mode
3. Try wordlist
4. Try rules
5. Try single mode
6. Try incremental
7. Use correct format
```

---
