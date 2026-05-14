# Android Hacking Phase 2 — APK Reverse Engineering & Static Analysis (CEH Practical Notes)

## Objective

Learn Android APK reverse engineering and static analysis using:

- apktool
    
- JADX
    
- AndroidManifest.xml analysis
    
- Smali inspection
    
- source code review
    
- vulnerability discovery
    

These are core Android pentesting skills used in:

- CEH Practical
    
- mobile application pentesting
    
- Android malware analysis
    
- bug bounty hunting
    
- APK security auditing
    

Android static analysis examines APKs without executing them and is one of the first stages of Android security assessment. ([HackWiki](https://hackwiki.com/09-mobile-security/android/02-static-analysis.html?utm_source=chatgpt.com "Step 2 - Android Static Analysis - Mobile Security | HackWiki"))

---

# 1. APK Reverse Engineering Workflow

## Standard Android Static Analysis Workflow

```text
APK → apktool → Manifest/Smali
APK → JADX → Java Source
```

---

# 2. Important Tools

|Tool|Purpose|
|---|---|
|apktool|decode APK resources + smali|
|JADX|decompile Java source|
|baksmali|disassemble DEX bytecode|
|dex2jar|convert DEX to JAR|
|APKiD|detect obfuscation/protection|

Android reverse engineering commonly uses apktool and JADX together because they solve different problems. ([REMnux Documentation](https://docs.remnux.org/discover-the-tools/statically%2Banalyze%2Bcode/android?utm_source=chatgpt.com "Android | REMnux Documentation"))

---

# 3. Pull APK from Device

## Find APK Path

```bash
adb shell pm path jakhar.aseem.diva
```

### Example Output

```bash
package:/data/app/jakhar.aseem.diva-1/base.apk
```

---

## Pull APK

```bash
adb pull /data/app/jakhar.aseem.diva-1/base.apk
```

---

## Rename APK

```bash
mv base.apk diva.apk
```

---

# 4. Verify Required Tools

## Verify apktool

```bash
apktool
```

---

## Verify JADX

```bash
jadx --version
```

---

## Verify Java

```bash
java -version
```

---

# 5. Install Missing Tools

## Install apktool

```bash
sudo apt install apktool
```

---

## Install JADX

```bash
sudo apt install jadx
```

---

# 6. APK Decompilation with apktool

APKs are ZIP archives containing:

- AndroidManifest.xml
    
- classes.dex
    
- resources
    
- native libraries
    

apktool decodes these resources into readable form. ([apktool.org](https://apktool.org/wiki/the-basics/intro/?utm_source=chatgpt.com "Introduction | Apktool"))

---

## Decompile APK

```bash
apktool d diva.apk
```

### Example Output

```text
I: Using Apktool...
I: Decoding AndroidManifest.xml
I: Baksmaling classes.dex...
```

---

## Decompiled Folder Structure

```text
diva/
├── AndroidManifest.xml
├── apktool.yml
├── assets/
├── lib/
├── res/
├── smali/
```

---

# 7. Important APK Components

|Component|Purpose|
|---|---|
|AndroidManifest.xml|app configuration|
|classes.dex|Dalvik bytecode|
|smali/|disassembled code|
|res/|resources|
|assets/|bundled files|
|lib/|native libraries|
|META-INF/|APK signatures|

APK structure and manifest analysis are core Android static-analysis concepts. ([HackWiki](https://hackwiki.com/09-mobile-security/android/02-static-analysis.html?utm_source=chatgpt.com "Step 2 - Android Static Analysis - Mobile Security | HackWiki"))

---

# 8. AndroidManifest.xml Analysis

The MOST IMPORTANT file during initial static analysis.

---

## Open Manifest

```bash
cat AndroidManifest.xml
```

OR:

```bash
less AndroidManifest.xml
```

Quit:

```text
q
```

---

# 9. Analyze Permissions

## Find Permissions

```bash
grep -i permission AndroidManifest.xml
```

---

## Dangerous Permissions

|Permission|Risk|
|---|---|
|READ_SMS|SMS theft|
|READ_CONTACTS|contact leakage|
|READ_CALL_LOG|call history exposure|
|RECORD_AUDIO|microphone access|
|CAMERA|camera access|
|WRITE_EXTERNAL_STORAGE|file overwrite|
|INTERNET|network communication|

---

# 10. Debuggable Application Detection

## Search Debuggable Flag

```bash
grep -i debuggable AndroidManifest.xml
```

### Dangerous Result

```xml
android:debuggable="true"
```

---

## Why Dangerous?

Allows:

- debugger attachment
    
- runtime analysis
    
- easier reverse engineering
    
- easier Frida hooking
    

Testing whether an app is debuggable is an OWASP Mobile Security testing category. ([OWASP Mobile Application Security](https://mas.owasp.org/MASTG/tools/android/MASTG-TOOL-0018/?utm_source=chatgpt.com "MASTG-TOOL-0018: jadx - OWASP Mobile Application Security"))

---

# 11. Backup Misconfiguration

## Search allowBackup

```bash
grep -i allowBackup AndroidManifest.xml
```

### Dangerous Result

```xml
android:allowBackup="true"
```

---

## Why Dangerous?

Possible:

- data extraction
    
- ADB backup abuse
    
- credential theft
    

---

# 12. Exported Component Enumeration

Exported Android components are one of the MOST IMPORTANT Android attack surfaces. ([OWASP Mobile Application Security](https://mas.owasp.org/MASTG/tools/android/MASTG-TOOL-0018/?utm_source=chatgpt.com "MASTG-TOOL-0018: jadx - OWASP Mobile Application Security"))

---

## Search Exported Components

```bash
grep -i exported AndroidManifest.xml
```

---

## Dangerous Result

```xml
android:exported="true"
```

---

## Components to Inspect

|Component|Risk|
|---|---|
|exported activities|auth bypass|
|exported providers|data leakage|
|exported receivers|broadcast abuse|
|exported services|unauthorized actions|

---

# 13. Search Intent Filters

## Search Intent Filters

```bash
grep -i intent-filter AndroidManifest.xml
```

---

## Why Important?

Intent filters may expose:

- hidden functionality
    
- deep links
    
- internal activities
    

---

# 14. Explore Smali Code

Smali = Android Dalvik bytecode assembly.

Smali maps closely to original DEX bytecode. ([Reddit](https://www.reddit.com/r/AskNetsec/comments/q60g8j/why_is_android_smali_code_reversible_and_jadx_one/?utm_source=chatgpt.com "Why is android smali code reversible and JADX one not"))

---

## Enter Smali Folder

```bash
cd smali
```

---

## List Packages

```bash
ls
```

---

# 15. Search Decompiled Code for Secrets

## Search Passwords

```bash
grep -R "password" .
```

---

## Search URLs

```bash
grep -R "http" .
```

---

## Search Tokens

```bash
grep -R "token" .
```

---

## Search Secrets

```bash
grep -R "secret" .
```

---

## Search API Keys

```bash
grep -R "key" .
```

Hardcoded secrets and URLs are common Android security findings. ([HackWiki](https://hackwiki.com/09-mobile-security/android/02-static-analysis.html?utm_source=chatgpt.com "Step 2 - Android Static Analysis - Mobile Security | HackWiki"))

---

# 16. JADX — Java Source Decompilation

JADX converts DEX bytecode into readable Java source code. ([OWASP Mobile Application Security](https://mas.owasp.org/MASTG/tools/android/MASTG-TOOL-0018/?utm_source=chatgpt.com "MASTG-TOOL-0018: jadx - OWASP Mobile Application Security"))

---

## Open JADX GUI

```bash
jadx-gui diva.apk
```

---

## Advantages of JADX

|Feature|Benefit|
|---|---|
|Java decompilation|readable code|
|code search|quick vulnerability hunting|
|cross-references|trace methods|
|GUI navigation|easier reversing|

---

# 17. Common Searches in JADX

## Search Hardcoded Credentials

```text
password
admin
secret
```

---

## Search URLs/APIs

```text
http
https
api
```

---

## Search Crypto Usage

```text
AES
DES
Cipher
```

---

## Search Logging

```text
Log.d
Log.e
System.out
```

---

## Search Storage APIs

```text
SharedPreferences
SQLiteDatabase
```

---

# 18. Important Vulnerability Indicators

|Indicator|Possible Vulnerability|
|---|---|
|hardcoded passwords|credential leakage|
|insecure HTTP|MITM|
|weak crypto|insecure encryption|
|exported activity|auth bypass|
|exported provider|data leakage|
|logging sensitive data|information disclosure|

---

# 19. Important DIVA Activities

|Activity|Vulnerability|
|---|---|
|HardcodeActivity|hardcoded credentials|
|LogActivity|sensitive logs|
|SQLInjectionActivity|SQL injection|
|InsecureDataStorage*|insecure storage|
|AccessControl*|auth bypass|

---

# 20. Important InsecureBankv2 Findings

|Finding|Severity|
|---|---|
|debuggable app|High|
|allowBackup enabled|Medium|
|exported activities|High|
|exported provider|Critical|
|exported receiver|Medium|

---

# 21. apktool vs JADX

|Feature|apktool|JADX|
|---|---|---|
|Smali output|✅|❌|
|Java source|❌|✅|
|Resource decoding|✅|Partial|
|APK rebuild|✅|❌|
|Easy readability|❌|✅|

Most Android researchers use BOTH tools together. ([AppSec Santa](https://appsecsanta.com/mobile-security-tools/apktool-vs-jadx?utm_source=chatgpt.com "Apktool vs JADX 2026: Android Reversing Toolchains"))

---

# 22. Most Important Commands to Memorize

## APK Extraction

```bash
adb shell pm path <package>
adb pull <apk-path>
```

---

## apktool

```bash
apktool d app.apk
```

---

## JADX

```bash
jadx-gui app.apk
```

---

## Manifest Analysis

```bash
grep -i permission AndroidManifest.xml
grep -i exported AndroidManifest.xml
grep -i debuggable AndroidManifest.xml
grep -i allowBackup AndroidManifest.xml
```

---

## Secret Hunting

```bash
grep -R "password" .
grep -R "http" .
grep -R "secret" .
grep -R "token" .
```

---

# 23. CEH Practical Relevance

These skills are used for:

- APK reversing
    
- static analysis
    
- malware analysis
    
- Android vulnerability assessment
    
- source code review
    
- mobile bug bounty
    
- exported component discovery
    
- secret extraction
    

---

# 24. Real-World Pentesting Workflow

```text
Pull APK
↓
apktool decode
↓
Manifest analysis
↓
JADX review
↓
Find attack surface
↓
Find secrets
↓
Dynamic analysis
```

Static analysis is typically combined with dynamic analysis tools like Frida and Burp Suite. ([arXiv](https://arxiv.org/abs/2604.14431?utm_source=chatgpt.com "AndroScanner: Automated Backend Vulnerability Detection for Android Applications"))

---

# 25. Phase 2 Summary

You learned:

✅ APK extraction  
✅ apktool usage  
✅ JADX usage  
✅ APK structure  
✅ Manifest analysis  
✅ permission analysis  
✅ exported component discovery  
✅ source code review  
✅ hardcoded secret hunting  
✅ Android attack surface enumeration

---

# Next Phase

## Phase 3 — Exported Component Exploitation

Topics:

- activity exploitation
    
- intent abuse
    
- broadcast injection
    
- content provider attacks
    
- authentication bypass
    
- Android IPC attacks
    

Android IPC and exported component testing are core OWASP Mobile Security testing areas. ([OWASP Mobile Application Security](https://mas.owasp.org/MASTG/tools/android/MASTG-TOOL-0018/?utm_source=chatgpt.com "MASTG-TOOL-0018: jadx - OWASP Mobile Application Security"))