# Android Hacking 101 – CEH Practical Notes

## Task 1 – Introduction

#android #ceh #mobilepentesting #apk #smali #reversing

---

# Android Application Types

## Native Applications

Applications developed specifically for:

- Android → Java / Kotlin
    
- iOS → Swift / Objective-C
    

### Features

- Better performance
    
- Direct hardware access
    
- OS-specific development
    

---

## Hybrid Applications

Applications built using:

- HTML
    
- CSS
    
- JavaScript
    

Frameworks:

- Apache Cordova / PhoneGap
    
- Ionic
    

### Features

- Cross-platform
    
- Faster development
    
- Web technologies inside mobile apps
    

---

# What is SMALI?

Smali is:

- Assembly language for Android Dalvik VM
    
- Human-readable representation of `.dex` bytecode
    

APK → classes.dex → Smali Code

---

# Important Smali Concepts

## Registers

Dalvik uses:

- 32-bit registers
    
- `long` and `double` use 2 registers
    

---

# Register Directives

## `.registers`

Specifies:

- Total number of registers
    

Example:

```smali
.registers 5
```

---

## `.locals`

Specifies:

- Only local registers
    
- Parameters excluded
    

Example:

```smali
.locals 2
```

---

# Parameter Passing

Method parameters are placed in:

- Last registers
    

Example:

```smali
.registers 5
```

Registers:

|Register|Purpose|
|---|---|
|v0|local|
|v1|local|
|v2|this / p0|
|v3|p1|
|v4|p2|

---

# p Naming Scheme

## Why Important?

Avoids renumbering when increasing registers.

Example:

```smali
p0 -> this
p1 -> first parameter
p2 -> second parameter
```

---

# Long / Double Registers

64-bit values require:

- 2 registers
    

Example:

```smali
(IJZ)V
```

|Register|Type|
|---|---|
|p0|this|
|p1|int|
|p2,p3|long|
|p4|boolean|

---

# APK Structure

|File/Folder|Purpose|
|---|---|
|AndroidManifest.xml|App metadata|
|classes.dex|Compiled app code|
|resources.arsc|Compiled resources|
|res/|App resources|
|assets/|Additional files|
|lib/|Native libraries|
|META-INF/|Signatures & metadata|

---

# AndroidManifest Important Tags

|Tag|Purpose|
|---|---|
|manifest|Package name & build info|
|permissions|Custom permissions|
|uses-permission|Requested permissions|
|uses-feature|Hardware/software usage|
|application|Main application|
|activity|UI screen|
|intent-filter|Intent handling|
|service|Background service|
|receiver|Broadcast receiver|
|provider|Content provider|

---

# CEH Exam Notes

## Remember

- `.dex` contains Dalvik bytecode
    
- Smali = assembly for Dalvik VM
    
- APK signature stored in:
    

```text
META-INF/
```

---

# Task 2 – Environment Setup

#adb #genymotion #android #ceh

---

# Required Tools

## Java Development Kit

Recommended:

```text
JDK 1.7
```

---

# Android Emulators

## Recommended Emulator

### Genymotion

Advantages:

- Fast
    
- Easy setup
    
- Root support
    

Recommended Android version:

```text
Android 6.0
```

Alternative:

- Nox Emulator
    

---

# Enable Developer Options

## Steps

```text
Settings → About Phone → Build Number
```

Tap:

```text
7 times
```

Then enable:

```text
Developer Options → USB Debugging
```

---

# ADB Basics

## View Connected Devices

```bash
adb devices
```

---

# Task 3 – Methodology

#methodology #pentest

---

# Android Pentesting Methodology

```text
Information Gathering
        ↓
Reversing APK
        ↓
Static Analysis
        ↓
Dynamic Analysis
        ↓
Traffic Analysis
        ↓
Exploitation
        ↓
Reporting
```

---

# Task 4 – Information Gathering

#informationgathering #playstore

---

# Black Box Testing

Tester has:

- No internal knowledge
    

### APK Collection

Usually from:

- Google Play Store
    

---

# White Box Testing

Client provides:

- APK
    
- Credentials
    
- User manuals
    
- Architecture details
    

---

# Important Note

⚠️ Never use online APK downloaders.

Reason:

- APK may be modified/trojanized
    

---

# CEH Exam Question

## Black Hat Europe Package Name

```text
com.swapcard.apps.android.blackhat
```

---

# Task 5 – Reversing

#reversing #jadx #apktool #dex2jar #adb

---

# Android Debug Bridge (ADB)

ADB allows:

- Communication with Android devices
    
- APK extraction
    
- Shell access
    

---

# Check Connected Devices

```bash
adb devices
```

Expected:

```text
List of devices attached
```

---

# Find APK Path

## Syntax

```bash
adb shell pm path <package_name>
```

## Example

```bash
adb shell pm path com.swapcard.apps.android.blackhat
```

Output:

```text
package:/data/app/com.swapcard.apps.android.blackhat-1/base.apk
```

---

# Extract APK

## Syntax

```bash
adb pull <remote_path>
```

## CEH Practical Command

```bash
adb pull /data/app/com.swapcard.apps.android.blackhat-1/base.apk
```

---

# JADX

## Purpose

- Decompile APK
    
- Convert Smali → Java
    

## Command

```bash
jadx -d output_folder app.apk
```

Example:

```bash
jadx -d output base.apk
```

---

# JADX GUI

## Launch GUI

```bash
jadx-gui
```

OR

```bash
jadx/bin/jadx-gui
```

---

# APKTool

## Decompile APK to Smali

```bash
apktool d app.apk
```

Example:

```bash
apktool d base.apk
```

---

# Build APK Using APKTool

## Build Option

```bash
b
```

## Example

```bash
apktool b app_folder
```

---

# Dex2Jar

## Convert APK → JAR

```bash
d2j-dex2jar.sh app.apk
```

Windows:

```bash
d2j-dex2jar.bat app.apk
```

---

# Dex → Smali Conversion

## Important CEH Answer

```text
d2j-dex2smali
```

---

# JD-GUI

## Purpose

Open `.jar` files and view Java source code.

Workflow:

```text
APK → Dex2Jar → JD-GUI
```

---

# Your Previously Used Commands (Revision)

#revision #practice #commands

---

# Check VBox Modules (Genymotion Fix)

```bash
lsmod | grep vbox
```

Expected modules:

```text
vboxdrv
vboxnetflt
vboxnetadp
```

---

# List Downloaded Files

```bash
ls -lh
```

---

# Search Installed Android Packages

```bash
adb shell pm list packages | grep -i black
```

---

# Find APK Path

```bash
adb shell pm path com.swapcard.apps.android.blackhat
```

---

# Pull APK

```bash
adb pull /data/app/com.swapcard.apps.android.blackhat-1/base.apk
```

---

# Find dex2jar Smali Tool

```bash
ls /opt/dex2jar/ | grep -i smali
```

---

# APK Reverse Engineering Workflow

```text
Install APK
      ↓
Find Package Name
      ↓
Get APK Path
      ↓
Pull APK
      ↓
jadx / apktool
      ↓
Analyze Source Code
```

---

# Most Important Commands for CEH Practical

## ADB

```bash
adb devices
adb shell
adb pull
adb push
adb install app.apk
adb uninstall package_name
```

---

## APKTool

```bash
apktool d app.apk
apktool b folder_name
```

---

## JADX

```bash
jadx -d output app.apk
jadx-gui
```

---

## Dex2Jar

```bash
d2j-dex2jar.sh app.apk
d2j-dex2smali
```

---

# Quick Viva Questions

## What contains Android bytecode?

```text
classes.dex
```

---

## Which folder stores APK signature?

```text
META-INF
```

---

## Tool for Smali code?

```text
apktool
```

---

## Tool for Java source code?

```text
jadx
```

---

## Tool to convert dex to jar?

```text
dex2jar
```

---

# Exam Tips

## During CEH Practical

1. Always identify package name first
    
2. Pull original APK from device
    
3. Use JADX for quick analysis
    
4. Use APKTool for smali modifications
    
5. Remember APK build command:
    

```bash
apktool b
```

---

# Cheatsheet

```bash
# View devices
adb devices

# Find package
adb shell pm list packages

# Find APK path
adb shell pm path package_name

# Pull APK
adb pull /path/base.apk

# Decompile APK
apktool d app.apk

# Build APK
apktool b folder

# Decompile to Java
jadx -d out app.apk

# GUI mode
jadx-gui

# APK → JAR
d2j-dex2jar.sh app.apk
```