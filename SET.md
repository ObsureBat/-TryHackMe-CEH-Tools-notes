# 🎣 Phishing – Merry Clickmas (CEH Practical Notes)

---

## 🔰 Overview

**Attack Type:** Social Engineering → Phishing  
**Tool Used:** Social-Engineer Toolkit (SET)  
**Goal:** Steal user credentials via fake login page

---

## 🎯 Learning Objectives

- Understand social engineering
    
- Types of phishing
    
- Create fake login page
    
- Send phishing email using SET
    
- Capture credentials
    

---

# 🧠 Social Engineering

---

## 📖 Definition

> Manipulating humans to reveal sensitive information

---

## 🎯 Common Tricks

- ⚡ Urgency (“Act now!”)
    
- 🎁 Temptation (“Free reward”)
    
- 👮 Authority (“Admin request”)
    
- 🤔 Curiosity (“Check this”)
    

---

### 🧩 Diagram: Social Engineering Flow

```
Attacker → Psychological Trick → Victim → Action → Compromise
```

---

# 🎣 Phishing

---

## 📖 Definition

> Fake communication to trick users into giving credentials/data

---

## 🔥 Types

|Type|Description|
|---|---|
|Email phishing|Fake emails|
|Smishing|SMS|
|Vishing|Calls|
|Quishing|QR codes|
|Social media|DMs|

---

# 🛡️ Anti-Phishing (S.T.O.P)

---

## 🧠 Check Before Clicking

### ✔ S.T.O.P (Detection)

- Suspicious?
    
- Telling me to click?
    
- Offering deal?
    
- Pushing urgency?
    

---

### ✔ S.T.O.P (Prevention)

- Slow down
    
- Type URL manually
    
- Open nothing unexpected
    
- Prove sender
    

---

# 🪤 Attack Setup (Fake Login Page)

---

## 📂 Navigate to Script

```bash
cd ~/Rooms/AoC2025/Day02
```

---

## ▶️ Run Phishing Server

```bash
./server.py
```

---

### 📊 Output

```
Starting server on http://0.0.0.0:8000
```

---

## 🌐 Access Page

```
http://<ATTACKER_IP>:8000
```

Example:

```
http://10.49.91.158:8000
```

---

### 🧠 What It Does

✔ Hosts fake login page  
✔ Captures credentials  
✔ Displays in terminal

---

### 🧩 Diagram: Credential Harvesting

```
Victim → Fake Login Page → Enters Credentials → Attacker Terminal
```

---

# 📧 Phase: Sending Phishing Email (SET)

---

## ▶️ Start SET

```bash
setoolkit
```

---

## 🔥 Menu Flow (IMPORTANT)

```
1 → Social-Engineering Attacks
5 → Mass Mailer Attack
1 → Single Email Attack
```

---

### 🧩 Diagram

```
SET → Social Engineering → Mailer → Email Delivery
```

---

# ⚙️ SET Configuration (EXAM IMPORTANT)

---

## 📌 Email Setup

|Field|Value|
|---|---|
|Target Email|[factory@wareville.thm](mailto:factory@wareville.thm)|
|From Email|[updates@flyingdeer.thm](mailto:updates@flyingdeer.thm)|
|From Name|Flying Deer|
|SMTP Server|10.49.180.188|
|Port|25|

---

## 📌 Options

- High Priority → No
    
- Attach File → No
    
- Inline File → No
    

---

## 📌 Email Content

### Subject:

```
Shipping Schedule Changes
```

---

### Body Example:

```
Dear elves,
Please confirm new schedule:
http://10.49.91.158:8000
Regards,
Flying Deer
END
```

---

### 🧠 Key Trick

✔ Include phishing link  
✔ Make email realistic

---

### 🧩 Diagram: Phishing Delivery

```
Attacker → SET → Email → Victim → Click Link → Fake Page
```

---

# 🧠 Phase: Capture Credentials

---

## 👀 Monitor Terminal

After sending email:

```
./server.py terminal
```

---

### 🎯 Result

✔ Victim enters credentials  
✔ Credentials captured

---

## 🔑 Password Found

```
unranked-wisdom-anthem
```

---

# 📬 Phase: Login to Target Portal

---

## 🌐 Access Mail Portal

```
http://10.49.180.188
```

---

## 🔑 Use Credentials

✔ Login successful

---

### 🎯 Data Extracted

```
Total toys = 1984000
```

---

# 🧠 Full Attack Flow (VERY IMPORTANT)

---

```
1. Setup phishing server
2. Create fake login page
3. Start SET tool
4. Configure phishing email
5. Send email to victim
6. Victim clicks link
7. Victim enters credentials
8. Capture credentials
9. Login to real system
10. Extract sensitive data
```

---

# 🔥 Commands Cheat Sheet

---

```bash
# Start phishing server
cd ~/Rooms/AoC2025/Day02
./server.py

# Start SET
setoolkit
```

---

# ⚠️ Common Mistakes

---

❌ Wrong IP in phishing link  
❌ Poor email wording  
❌ Not monitoring terminal  
❌ Wrong SMTP settings

---

# 🚀 CEH Practical Tips

---

## 🎯 Key Focus Areas

- SET tool usage
    
- Social engineering concepts
    
- Phishing workflow
    
- Credential harvesting
    

---

## 🧠 Exam Strategy

1. Always **create believable email**
    
2. Use **trusted-looking sender**
    
3. Host fake login page
    
4. Capture credentials
    
5. Try **credential reuse**
    

---

# 🧾 Quick Revision Table

---

|Step|Tool|
|---|---|
|Fake page|server.py|
|Email sending|SET|
|Credential capture|Terminal|
|Login|Browser|

---

# 🧩 Final Concept Diagram

```
Attacker
   ↓
Fake Login Page (server.py)
   ↓
SET Phishing Email
   ↓
Victim Clicks Link
   ↓
Credentials Captured
   ↓
Login to Target System
   ↓
Data Extraction
```

---
