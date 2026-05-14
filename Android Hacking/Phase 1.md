# Android Hacking Phase 1 — ADB & Android Filesystem Mastery (CEH Practical Notes)

## Objective

Learn the foundational Android pentesting workflow using:

- ADB
    
- Android shell
    
- filesystem navigation
    
- package enumeration
    
- app data extraction
    
- SQLite inspection
    

These are core skills for:

- CEH Practical
    
- Android pentesting
    
- Mobile app analysis
    
- APK reversing
    

Android Debug Bridge (ADB) is the official Android command-line interface for device communication and debugging.

---

# 1. Verify ADB Connection

## List Connected Devices

```bash
adb devices
```

### Expected Output

```bash
List of devices attached
192.168.x.x:5555 device
```

---

## Restart ADB Server

```bash
adb kill-server
adb start-server
```

---

## Connect to Emulator Manually

```bash
adb connect <IP>:5555
```

### Example

```bash
adb connect 192.168.56.101:5555
```

---

# 2. Open Android Shell

```bash
adb shell
```

### Example Prompt

```bash
vbox86p:/ $
```

Android shell is Linux-based.

---

# 3. Basic Linux Navigation Inside Android

## Present Working Directory

```bash
pwd
```

---

## List Files

```bash
ls
```

---

## Change Directory

```bash
cd /sdcard
```

---

## Create Directory

```bash
mkdir test
```

---

## Create File

```bash
touch hello.txt
```

---

## Read File

```bash
cat hello.txt
```

---

## Find Files

```bash
find . -name "*.apk"
```

---

# 4. Important Android Directories

## Shared Storage

```bash
/sdcard
```

### Contains

- Downloads
    
- Pictures
    
- Documents
    
- APKs
    

---

## Application Private Storage

```bash
/data/data
```

### Contains

- databases
    
- shared preferences
    
- cache
    
- internal files
    

Most sensitive Android data is stored here.

---

## Installed APK Locations

```bash
/data/app
```

Contains installed application APKs.

---

# 5. File Transfer with ADB

## Push File to Android

```bash
adb push hello.txt /sdcard/
```

---

## Pull File from Android

```bash
adb pull /sdcard/hello.txt
```

---

# 6. APK Installation

## Install APK

```bash
adb install app.apk
```

---

## Reinstall Existing APK

```bash
adb install -r app.apk
```

---

# 7. Package Enumeration

## List All Packages

```bash
adb shell pm list packages
```

---

## List Third-Party Apps Only

```bash
adb shell pm list packages -3
```

### Example

```bash
package:com.android.insecurebankv2
package:jakhar.aseem.diva
```

---

## List System Apps

```bash
adb shell pm list packages -s
```

---

## Search Package

```bash
adb shell pm list packages | grep diva
```

---

# 8. Find APK Path

## Find Installed APK Location

```bash
adb shell pm path jakhar.aseem.diva
```

### Example Output

```bash
package:/data/app/jakhar.aseem.diva-1/base.apk
```

---

# 9. Extract APK

## Pull APK from Device

```bash
adb pull /data/app/jakhar.aseem.diva-1/base.apk
```

---

# 10. Log Analysis

## View Android Logs

```bash
adb logcat
```

---

## Search Logs for Passwords

```bash
adb logcat | grep -i password
```

---

# 11. SharedPreferences Analysis

## Navigate to App Storage

```bash
adb shell
cd /data/data/jakhar.aseem.diva
```

---

## List Files

```bash
ls
```

### Example Output

```bash
cache
code_cache
databases
shared_prefs
```

---

## Open Shared Preferences

```bash
cd shared_prefs
ls
```

---

## Read XML Preferences

```bash
cat jakhar.aseem.diva_preferences.xml
```

### Example Output

```xml
<?xml version='1.0' encoding='utf-8' standalone='yes' ?>
<map>
    <string name="password">test1</string>
    <string name="user">test1</string>
</map>
```

---

# 12. SQLite Database Analysis

SQLite is the default embedded database engine used by Android apps.

---

## Navigate to Database Folder

```bash
cd /data/data/jakhar.aseem.diva/databases
```

---

## List Databases

```bash
ls
```

### Example

```bash
divanotes.db
divanotes.db-journal
```

---

## Open SQLite Database

```bash
sqlite3 divanotes.db
```

---

## Show Tables

```sql
.tables
```

### Example Output

```sql
android_metadata
notes
```

---

## Show Database Schema

```sql
.schema
```

---

## Dump Table Data

```sql
SELECT * FROM notes;
```

### Example Output

```sql
1|office|10 Meetings. 5 Calls. Lunch with CEO
2|home|Buy toys for baby, Order dinner
3|holiday|Either Goa or Amsterdam
```

---

## Exit SQLite

```sql
.exit
```

---

# 13. Important Android Storage Locations

|Directory|Purpose|
|---|---|
|`/sdcard`|Shared storage|
|`/data/app`|Installed APKs|
|`/data/data`|App private data|
|`shared_prefs`|XML preferences|
|`databases`|SQLite databases|
|`cache`|Temporary files|

---

# 14. Common Android Vulnerabilities Found in Storage

## Insecure SharedPreferences

Sensitive data stored in plaintext XML.

---

## Unencrypted SQLite Databases

Credentials/tokens stored without encryption.

---

## Excessive Logging

Sensitive information visible in `logcat`.

---

## World-Readable Files

Improper file permissions exposing data.

---

# 15. Most Important Commands to Memorize

## ADB Basics

```bash
adb devices
adb shell
adb push
adb pull
adb install
adb logcat
```

---

## Package Enumeration

```bash
adb shell pm list packages
adb shell pm list packages -3
adb shell pm path <package>
```

---

## SQLite

```bash
sqlite3 database.db
```

```sql
.tables
.schema
SELECT * FROM notes;
.exit
```

---

# 16. CEH Practical Relevance

These skills are used for:

- Android forensic analysis
    
- application pentesting
    
- credential extraction
    
- APK extraction
    
- vulnerable app analysis
    
- Drozer exploitation
    
- MobSF analysis
    
- reverse engineering preparation
    

---

# 17. Tools Used in Phase 1

|Tool|Purpose|
|---|---|
|ADB|Device communication|
|sqlite3|Database analysis|
|Android shell|Filesystem interaction|

---

# 18. Vulnerable Apps Used

## Damn Insecure and Vulnerable App (DIVA)

Used for:

- insecure storage
    
- SQLi practice
    
- Android vulnerability analysis
    

---

## InsecureBankv2

Used for:

- Android banking app pentesting
    
- exported component analysis
    
- APK reversing
    
- dynamic analysis
    

---

# 19. Phase 1 Summary

You learned:

✅ ADB basics  
✅ Android shell navigation  
✅ Package enumeration  
✅ APK extraction  
✅ SharedPreferences analysis  
✅ SQLite database analysis  
✅ Android filesystem structure  
✅ Sensitive data extraction  
✅ Android app storage methodology

---

# Next Phase

## Phase 2 — APK Reverse Engineering

Topics:

- APK decompilation
    
- apktool
    
- JADX
    
- AndroidManifest.xml
    
- exported activities
    
- hardcoded secrets
    
- permission analysis
    
- static analysis
    

ADB and Android storage behavior are officially documented by Android Developers and SQLite documentation.