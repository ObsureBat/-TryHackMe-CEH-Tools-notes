# 🛡️ OpenVAS (GVM) — Complete Revision Notes

---

## 🔹 1. What is OpenVAS?

- **OpenVAS**  
    → Open-source vulnerability scanner used to:
    
    - Detect system & network vulnerabilities
        
    - Scan web applications
        
    - Identify misconfigurations
        
- Part of **Greenbone Vulnerability Management**
    

💡 **Important:**

- It **does NOT exploit vulnerabilities**
    
- It only **detects & reports**
    

---

## 🔹 2. GVM Framework Architecture (VERY IMPORTANT ⚡)

OpenVAS works inside the **GVM framework**, divided into:

---

### 🧱 1. Front-End (User Interface)

- **Greenbone Security Assistant**
    
- Web UI for:
    
    - Managing scans
        
    - Viewing reports
        
    - Configuring tasks
        

---

### ⚙️ 2. Back-End (Core Engine)

- OpenVAS Scanner
    
- OSP (Open Scanner Protocol)
    
- Targets
    

👉 Handles:

- Running scans
    
- Processing vulnerabilities
    

---

### 📡 3. Vulnerability Feed

- NVTs (Network Vulnerability Tests)
    
- SCAP / CERT data
    
- Community feed
    

👉 Contains:

- Updated vulnerability database
    
- Detection logic
    

---

## 🔹 3. Installation Methods

### 🥇 Best Method: Docker (Recommended)

```bash
apt-get update && apt-get upgrade
apt-get install docker.io

docker run -d -p 443:443 --name openvas mikesplain/openvas
```

👉 Access:

```
https://127.0.0.1
Username: admin
Password: admin
```

---

### 🐳 Docker Management

```bash
docker ps -a        # List containers
docker start <id>   # Start container
```

---

### ⚠️ Other Methods

- Kali repo → unstable sometimes
    
- Source install → complex ❌
    

---

## 🔹 4. OpenVAS Workflow (IMPORTANT ⚡)

### Step 1: Create Target

- Go → **Scan Targets**
    
- Add:
    
    - Name
        
    - Target IP
        

---

### Step 2: Create Task

- Go → **Scans → Tasks → New Task**
    

#### Key Fields:

- **Name** → Scan name
    
- **Scan Targets** → Selected target
    
- **Scanner** → OpenVAS default
    
- **Scan Config** → Scan type
    

---

### Step 3: Start Scan

- Click ▶ (Start button)
    

---

## 🔹 5. Scan Configurations

OpenVAS provides multiple scan types (similar to Nessus):

|Scan Type|Purpose|
|---|---|
|Full & Fast|Default (most used)|
|Full & Very Deep|Detailed scan|
|Host Discovery|Find alive hosts|
|System Discovery|Basic enumeration|

💡 **Exam Tip:**  
👉 Default = **Full and Fast**

---

## 🔹 6. Practical Case Study (IMPORTANT ⚡)

### 📊 Scan Details

- **Start Time:** Feb 28, 00:04:46
    
- **End Time:** Feb 28, 00:21:02
    

---

### 🌐 Network Info

- **Open Ports:** 3
    

---

### ⚠️ Vulnerabilities

- **Total Found:** 5
    
- **Highest Severity:**
    
    - **MS17-010 (EternalBlue)**
        

---

### 💻 Affected OS

- **Microsoft Windows 10 (x32/x64)**
    

---

### 🔍 Detection Method

- Send crafted SMB request:
    
    - `fid = 0`
        
- Analyze response
    

---

## 🔹 7. Critical Vulnerability — MS17-010

### 🧨 What is it?

- SMB vulnerability (EternalBlue)
    
- Used in:
    
    - WannaCry ransomware
        

---

### 🔗 Works With:

- Metasploit exploit module
    

---

## 🔹 8. Key OpenVAS Concepts

### 🧠 NVT (Network Vulnerability Tests)

- Scripts used to:
    
    - Detect vulnerabilities
        
    - Check configurations
        

---

### 📊 Scan Reports Include:

- Severity
    
- CVE references
    
- Description
    
- Fix recommendations
    

---

## 🔹 9. OpenVAS vs Nessus (Exam Comparison ⚡)

|Feature|OpenVAS|Nessus|
|---|---|---|
|Type|Open-source|Commercial|
|Feed|Community|Proprietary|
|Ease|Moderate|Easy|
|Accuracy|Good|Very High|

---

## 🔹 10. CEH Practical Quick Points 🚀

- OpenVAS = **scanner only (no exploitation)**
    
- Uses:
    
    - NVTs
        
    - GVM framework
        
- Workflow:
    
    1. Target
        
    2. Task
        
    3. Scan
        
- Important vuln:
    
    - **MS17-010**
        
- Default scan:
    
    - **Full & Fast**
        

---

## 🔹 11. Real Exam Scenarios

If question says:

- “Scan vulnerabilities?” → OpenVAS
    
- “Exploit MS17-010?” → Metasploit
    
- “Detection script?” → NVT
    
- “UI interface?” → GSA
    

---

## 🔥 Final 30-Second Revision

- Tool: **OpenVAS (GVM)**
    
- UI: **Greenbone Security Assistant**
    
- Scan steps:
    
    - Target → Task → Start
        
- Default scan:
    
    - **Full & Fast**
        
- Key vuln:
    
    - **MS17-010**
        
- Ports open:
    
    - **3**
        
- Total vulns:
    
    - **5**
        

---
