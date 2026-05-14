# 🛡️ Nessus — Quick Revision Notes (CEH Practical)

## 🔹 1. What is Nessus?

- Developed by Tenable
    
- A **vulnerability scanner** used to:
    
    - Detect misconfigurations
        
    - Identify vulnerabilities
        
    - Audit systems & networks
        

### Versions:

- **Nessus Essentials** → Free (limited hosts)
    
- **Nessus Professional / Manager** → Advanced enterprise features
    

---

## 🔹 2. Key Interface & Navigation

### 🧭 Important UI Sections

- **New Scan** → Launch scans
    
- **Policies** → Create custom scan templates
    
- **Plugin Rules** → Modify plugin behavior (severity, hide, etc.)
    
- **Scans Tab** → View & manage scans
    

---

## 🔹 3. Common Scan Types (VERY IMPORTANT ⚡)

|Scan Type|Purpose|
|---|---|
|**Host Discovery**|Check which hosts are alive|
|**Basic Network Scan**|General-purpose scan (MOST USED)|
|**Credentialed Patch Audit**|Authenticated scan for missing updates|
|**Web Application Tests**|Scan web apps for vulnerabilities|

💡 **Exam Tip:**  
👉 If confused → always choose **Basic Network Scan**

---

## 🔹 4. Scan Configuration Options

### 🧱 BASIC Tab

- **Schedule** → Run scan later (useful for congestion)
    

### 🔍 DISCOVERY Tab

- **Port Scan (All Ports)** → Scans ports **1–65535**
    

### ⚙️ ADVANCED Tab

- **Scan low bandwidth links** → For slow networks
    

---

## 🔹 5. Running a Scan

### Steps:

1. Click **New Scan**
    
2. Choose **Basic Network Scan**
    
3. Enter:
    
    - Target IP
        
4. Configure options
    
5. Click **Save → Launch**
    

---

## 🔹 6. Important Scan Results (HIGH VALUE ⚡)

### 🔍 Port Scanning Plugin

- **Nessus SYN Scanner**
    
    - Shows **open ports**
        
    - Located in **Port Scanners family**
        

---

### 🌐 Web Server Info

- Apache version detected:
    
    - **2.4.99**
        

---

### 🔌 Key Plugin Info

- Plugin ID for HTTP server detection:
    
    - **10107**
        

---

## 🔹 7. Vulnerability Findings (EXAM GOLD ⚡)

### 🔑 Weak Authentication

- Page:
    
    - `login.php`
        
- Issue:
    
    - Credentials sent in **cleartext**
        

---

### 📂 Backup File Disclosure

- Extension:
    
    - **.bak**
        

---

### 📁 Exposed Directory

- Path:
    
    ```
    /external/phpids/0.6/docs/examples/
    ```
    

---

### 🛑 Clickjacking Vulnerability

- Cause:
    
    - Missing **X-Frame-Options header**
        
- Attack:
    
    - Trick user into clicking hidden elements
        

---

## 🔹 8. Important Concepts

### 🧠 Plugins

- Nessus uses **plugins** to detect vulnerabilities
    
- Each plugin has:
    
    - ID
        
    - Severity
        
    - Description
        

---

### 📊 Severity Levels

- Critical
    
- High
    
- Medium
    
- Low
    
- Info
    

---

## 🔹 9. CEH Practical Quick Commands/Concepts

### Common Things to Remember:

- Launch scan → **New Scan**
    
- Templates → **Policies**
    
- Modify plugins → **Plugin Rules**
    
- Port scan full → **1–65535**
    
- Best scan → **Basic Network Scan**
    

---

## 🔹 10. Real Exam Tips 🚀

- If asked:
    
    - “Scan all ports?” → **Port scan (all ports)**
        
    - “Slow network?” → **Low bandwidth scan**
        
    - “Find open ports?” → **Nessus SYN scanner**
        
    - “Web vuln?” → Check:
        
        - login.php
            
        - .bak files
            
        - directories
            
        - clickjacking
            

---

## 🔹 11. Tools Integration (Important for CEH)

Nessus works well with:

- Nmap → Port scanning
    
- Metasploit → Exploitation
    
- Burp Suite → Web testing
    

---

## 🔥 Final 30-Second Revision

- Tool: **Nessus (Tenable)**
    
- Start scan: **New Scan**
    
- Best scan: **Basic Network Scan**
    
- Full ports: **1–65535**
    
- Plugin for ports: **Nessus SYN scanner**
    
- HTTP plugin ID: **10107**
    
- Vulns:
    
    - login.php (cleartext creds)
        
    - .bak files
        
    - exposed directories
        
    - clickjacking
        

---
