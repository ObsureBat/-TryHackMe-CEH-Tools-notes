# 🔐 VeraCrypt (Disk Encryption)

## 📌 Overview

- VeraCrypt is used to:
    
    - Create **encrypted containers**
        
    - Encrypt **entire disks / partitions**
        
- Common in CEH for:
    
    - Detecting encrypted volumes
        
    - Mounting containers (if password known)
        

---

## ⚙️ Installation (Kali Linux)

```bash
sudo apt update
sudo apt install veracrypt -y
```

---

## 🚀 Launch VeraCrypt

```bash
veracrypt
```

---

## 📦 Create Encrypted Volume (GUI Steps)

1. Create Volume
    
2. Create an encrypted file container
    
3. Select Standard VeraCrypt volume
    
4. Choose file location
    
5. Select encryption (AES default)
    
6. Set password
    
7. Format volume
    

---

## 🔓 Mount Volume (CLI Method)

```bash
veracrypt --text --mount /path/to/container.hc /mnt/veracrypt
```

👉 Example:

```bash
veracrypt --text --mount secret.hc /mnt/veracrypt
```

---

## 🔒 Dismount Volume

```bash
veracrypt -d /mnt/veracrypt
```

OR

```bash
veracrypt -d
```

---

## 🔍 Check Mounted Volumes

```bash
veracrypt -l
```

---

## 🧠 CEH Practical Tips

- If you see `.hc` file → likely VeraCrypt container
    
- Try:
    
    - Mounting with known password
        
    - Dictionary attack (rare in exam)
        
- Default mount path:
    

```bash
/mnt/veracrypt
```

---

---

# 🔑 Hash-Identifier

## 📌 Overview

- Hash-Identifier is used to:
    
    - Identify **hash type** (MD5, SHA1, NTLM, etc.)
        
- Used before cracking with:
    
    - Hashcat
        
    - John the Ripper
        

---

## ⚙️ Installation

```bash
sudo apt install hashid -y
```

---

## 🚀 Basic Usage

```bash
hashid <hash>
```

👉 Example:

```bash
hashid 5f4dcc3b5aa765d61d8327deb882cf99
```

---

## 🔍 Identify Multiple Hashes

```bash
hashid hashes.txt
```

---

## 📊 Extended Mode (More Details)

```bash
hashid -e <hash>
```

👉 Example:

```bash
hashid -e 5f4dcc3b5aa765d61d8327deb882cf99
```

---

## 📁 Using Hash Identifier Script (Alternative Tool)

```bash
python3 hash-identifier.py
```

Then paste hash.

---

## 🧠 Common Hash Lengths (IMPORTANT)

|Hash Type|Length|
|---|---|
|MD5|32|
|SHA1|40|
|SHA256|64|
|NTLM|32|

---

## 🔥 CEH Practical Workflow

1. Identify hash:
    

```bash
hashid <hash>
```

2. Crack using Hashcat:
    

```bash
hashcat -m <mode> hash.txt wordlist.txt
```

👉 Example (MD5):

```bash
hashcat -m 0 hash.txt rockyou.txt
```

---

## ⚠️ Exam Tips

- Always run:
    

```bash
hashid -e <hash>
```

- Then choose correct **Hashcat mode**
    
- Wrong mode = no result
    

---

---

# 🧠 Quick Revision Cheatsheet

## VeraCrypt

```bash
veracrypt
veracrypt --text --mount file.hc /mnt/veracrypt
veracrypt -d
veracrypt -l
```

---

## Hash-Identifier

```bash
hashid <hash>
hashid -e <hash>
hashid hashes.txt
```

---

# 🎯 Final CEH Practical Strategy

- **If encrypted file found → VeraCrypt**
    
- **If hash found → Hash-Identifier → Hashcat**
    
- Always:
    
    - Identify first
        
    - Then attack
        

---
**