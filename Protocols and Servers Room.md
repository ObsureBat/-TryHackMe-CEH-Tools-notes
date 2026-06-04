# TryHackMe — [Protocols and Servers Room](https://tryhackme.com/room/protocolsandservers?utm_source=chatgpt.com) Complete Summary + Diagrams + Answers

This room teaches you how common internet protocols work **under the hood** and why insecure versions are dangerous in cybersecurity. ([TryHackMe](https://tryhackme.com/room/protocolsandservers?utm_source=chatgpt.com "TryHackMe | Protocols and Servers"))

You learned:

- How clients communicate with servers
    
- How Telnet can manually interact with protocols
    
- How HTTP, FTP, SMTP, POP3, and IMAP work
    
- Why plaintext protocols are insecure
    
- Secure alternatives like HTTPS, SFTP, IMAPS, and SSH
    

---

# 1. Big Picture — What Are Protocols?

A **protocol** is a set of rules devices use to communicate.

Examples:

|Protocol|Purpose|Default Port|
|---|---|---|
|Telnet|Remote terminal access|23|
|HTTP|Websites|80|
|FTP|File transfer|21|
|SMTP|Sending email|25|
|POP3|Downloading email|110|
|IMAP|Synchronizing email|143|

---

# 2. Core Idea of This Room

The room focuses on one huge cybersecurity lesson:

# ⚠️ Cleartext = Dangerous

Older protocols send everything as readable text.

That means attackers can sniff:

- usernames
    
- passwords
    
- emails
    
- files
    
- commands
    

Example:

```bash
USER frank
PASS D2xc9CgD
```

Anyone monitoring the network can steal this instantly.

---

# 3. Telnet

## What is Telnet?

Telnet is an old remote access protocol.

Default port:

```text
23
```

But in cybersecurity, Telnet is useful because it can connect to **any TCP port**.

---

# Telnet Diagram

```text
+-------------+       TCP Connection       +-------------+
|   Attacker  | -------------------------> |   Server    |
|   Telnet    |                            | HTTP / FTP  |
+-------------+                            +-------------+
```

---

# Why Telnet Was Used in This Room

Because all these protocols are text-based.

You manually type protocol commands to understand what browsers and applications normally do automatically.

---

# Task 2 Answer

## Q. To which port does Telnet connect by default?

```text
23
```

([Medium](https://cursemagic.medium.com/tryhackme-protocols-and-servers-write-up-a5f1a4095c69?utm_source=chatgpt.com "TryHackMe: Protocols and Servers Write-up | by Cursemagic | Medium"))

---

# 4. HTTP — HyperText Transfer Protocol

## What is HTTP?

Hypertext Transfer Protocol is used for websites.

Browser → Server communication.

---

# HTTP Flow Diagram

```text
Browser ---- HTTP Request ----> Web Server
Browser <--- HTTP Response ---- Web Server
```

---

# Example Request

```http
GET /index.html HTTP/1.1
Host: telnet
```

---

# Example Response

```http
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
```

---

# Important Concepts

## GET Request

```http
GET /page
```

Requests a page/file.

---

## Headers

Example:

```http
Server: nginx/1.18.0
```

This leaks:

- web server software
    
- version
    
- OS
    

Useful during reconnaissance.

---

# HTTP vs HTTPS

|HTTP|HTTPS|
|---|---|
|Plaintext|Encrypted|
|Port 80|Port 443|
|Insecure|Secure|
|Sniffable|Protected|

---

# HTTP Diagram

```text
Client --> GET /index.html --> Server
Client <-- HTML Response ---- Server
```

---

# Task 3 Answer

## Q. Retrieve `flag.thm`

Use:

```bash
telnet MACHINE_IP 80
```

Then:

```http
GET /flag.thm HTTP/1.1
host: telnet
```

Press ENTER twice.

The server returns the flag.

([Medium](https://cursemagic.medium.com/tryhackme-protocols-and-servers-write-up-a5f1a4095c69?utm_source=chatgpt.com "TryHackMe: Protocols and Servers Write-up | by Cursemagic | Medium"))

---

# 5. FTP — File Transfer Protocol

## What is FTP?

File Transfer Protocol transfers files between systems.

Default port:

```text
21
```

---

# FTP Architecture

FTP uses TWO connections.

```text
Control Connection --> Commands/Login
Data Connection ----> File Transfer
```

---

# FTP Diagram

```text
             Port 21
Client -----------------> FTP Server
        Control Channel

             Random Port
Client <----------------> FTP Server
          Data Channel
```

---

# Important Commands

|Command|Purpose|
|---|---|
|USER|Username|
|PASS|Password|
|ls|List files|
|get|Download file|
|ascii|ASCII mode|
|binary|Binary mode|

---

# FTP Modes

## Active Mode

Server initiates data connection.

## Passive Mode

Client initiates everything.

Passive is firewall-friendly.

---

# Anonymous FTP

Sometimes login works with:

```text
anonymous
```

Huge penetration testing finding.

---

# FTP Security Problem

Everything is plaintext:

```text
USER frank
PASS D2xc9CgD
```

Attackers can steal credentials.

---

# Secure Alternatives

|Insecure|Secure|
|---|---|
|FTP|SFTP|
|FTP|FTPS|

---

# FTP File Transfer Flow

```text
1. Login
2. List files
3. Open data connection
4. Download file
```

---

# Task 4 Answer

Login credentials:

```text
Username: frank
Password: D2xc9CgD
```

Commands:

```bash
ftp MACHINE_IP
```

Then:

```bash
ls
get flag.thm
```

Open the downloaded file to see the flag.

([Medium](https://cursemagic.medium.com/tryhackme-protocols-and-servers-write-up-a5f1a4095c69?utm_source=chatgpt.com "TryHackMe: Protocols and Servers Write-up | by Cursemagic | Medium"))

---

# 6. SMTP — Sending Email

## What is SMTP?

Simple Mail Transfer Protocol sends email.

Default port:

```text
25
```

---

# Email Architecture

```text
MUA --> MSA --> MTA --> MDA --> MUA
```

---

# Easier Version

```text
Your Mail App
    ↓
Mail Server
    ↓
Recipient Mail Server
    ↓
Recipient Inbox
```

---

# SMTP Commands

|Command|Purpose|
|---|---|
|HELO/EHLO|Introduce client|
|MAIL FROM|Sender|
|RCPT TO|Recipient|
|DATA|Start message|
|.|End message|

---

# SMTP Diagram

```text
Client ---> SMTP Server ---> Recipient Server
```

---

# Email Spoofing

SMTP trusts sender input.

Example:

```text
MAIL FROM: president@company.com
```

SMTP originally did NOT verify ownership.

This is why phishing exists.

---

# SMTP Security Problems

- cleartext credentials
    
- spoofing
    
- sniffing
    
- spam abuse
    

---

# Secure Versions

|Protocol|Port|
|---|---|
|SMTPS|465|
|Submission + STARTTLS|587|

---

# Task 5 Answer

Connect:

```bash
telnet MACHINE_IP 25
```

Read the SMTP banner for the flag.

([TryHackMe](https://tryhackme.com/room/protocolsandservers?utm_source=chatgpt.com "TryHackMe | Protocols and Servers"))

---

# 7. POP3 — Downloading Email

## What is POP3?

Post Office Protocol downloads emails from the server.

Default port:

```text
110
```

---

# POP3 Workflow

```text
1. Login
2. Download emails
3. Usually delete from server
```

---

# POP3 Diagram

```text
Mail Server ---> Downloads ---> User Device
```

---

# POP3 Commands

|Command|Purpose|
|---|---|
|USER|Username|
|PASS|Password|
|STAT|Mailbox stats|
|LIST|List emails|
|RETR|Retrieve email|
|DELE|Delete email|

---

# STAT Output

Format:

```text
+OK <num_messages> <size>
```

Example:

```text
+OK 1 179
```

Means:

- 1 email
    
- 179 bytes
    

---

# POP3 Weakness

Poor synchronization.

If downloaded:

- email disappears from server
    
- only available on one device
    

---

# Security Problem

Credentials are plaintext.

Attackers sniff:

```text
USER frank
PASS D2xc9CgD
```

---

# Secure Version

```text
POP3S → Port 995
```

---

# Task 6 Answers

## Q1. What is the response to STAT?

```text
+OK 0 0
```

## Q2. How many emails are available?

```text
0
```

([Medium](https://cursemagic.medium.com/tryhackme-protocols-and-servers-write-up-a5f1a4095c69?utm_source=chatgpt.com "TryHackMe: Protocols and Servers Write-up | by Cursemagic | Medium"))

---

# 8. IMAP — Modern Email Synchronization

## What is IMAP?

Internet Message Access Protocol synchronizes email across devices.

Default port:

```text
143
```

---

# Why IMAP Became Standard

Unlike POP3:

✅ emails stay on server  
✅ syncs read/unread status  
✅ works across multiple devices  
✅ folder synchronization

---

# IMAP Diagram

```text
          +-------------+
Phone --->|             |
Laptop -->| IMAP Server |
Tablet -->|             |
          +-------------+
```

All devices stay synchronized.

---

# IMAP Commands

|Command|Purpose|
|---|---|
|LOGIN|Authenticate|
|LIST|Show folders|
|SELECT|Open mailbox|
|FETCH|Retrieve email|
|SEARCH|Search mail|

---

# IMAP Tags

Commands need IDs:

```text
c1 LOGIN
c2 LIST
c3 EXAMINE
```

Server uses them to match replies.

---

# IMAP Security Risk

Attackers gain:

- full mailbox history
    
- future emails
    
- reset links
    
- business data
    

---

# Secure Version

```text
IMAPS → Port 993
```

---

# Task 7 Answer

## Q. Default IMAP port?

```text
143
```

([TryHackMe](https://tryhackme.com/room/protocolsandservers?utm_source=chatgpt.com "TryHackMe | Protocols and Servers"))

---

# 9. MOST IMPORTANT CYBERSECURITY LESSON

# 🚨 Plaintext Protocols Are Dangerous

If traffic is not encrypted:

```text
Attacker ---> Sniffs Network ---> Sees Everything
```

This includes:

- passwords
    
- files
    
- emails
    
- cookies
    
- commands
    

---

# 10. Secure Alternatives

|Old Protocol|Secure Version|
|---|---|
|Telnet|SSH|
|HTTP|HTTPS|
|FTP|SFTP / FTPS|
|POP3|POP3S|
|IMAP|IMAPS|
|SMTP|SMTPS|

---

# Final Master Diagram

```text
                INTERNET PROTOCOLS

+---------+       +---------+       +---------+
| Browser | <---> |  HTTP   | <---> | WebSrv  |
+---------+       +---------+       +---------+

+---------+       +---------+       +---------+
| FTP Cli | <---> |   FTP   | <---> | FTPSrv  |
+---------+       +---------+       +---------+

+---------+       +---------+       +---------+
| MailApp | <---> |  SMTP   | <---> | MailSrv |
+---------+       +---------+       +---------+

+---------+       +---------+       +---------+
| MailApp | <---> | POP3 /  | <---> | MailSrv |
|         |       |  IMAP   |       |         |
+---------+       +---------+       +---------+
```

---

# Full Answers List

|Task|Answer|
|---|---|
|Telnet default port|23|
|HTTP flag|Retrieve via GET request|
|FTP flag|Download using get|
|SMTP flag|Banner output|
|POP3 STAT|+OK 0 0|
|POP3 mail count|0|
|IMAP default port|143|

([Medium](https://cursemagic.medium.com/tryhackme-protocols-and-servers-write-up-a5f1a4095c69?utm_source=chatgpt.com "TryHackMe: Protocols and Servers Write-up | by Cursemagic | Medium"))

---

# Best Way to Remember Everything

## Think Like This:

|Protocol|Real Life Analogy|
|---|---|
|HTTP|Visiting a website|
|FTP|File courier|
|SMTP|Sending letters|
|POP3|Downloading letters|
|IMAP|Syncing mailbox everywhere|
|Telnet|Manual remote terminal|

---

# Key Exam / Interview Concepts

## VERY IMPORTANT

### 1. Cleartext vs Encryption

Most common interview question.

### 2. Difference Between POP3 and IMAP

|POP3|IMAP|
|---|---|
|Downloads mail|Syncs mail|
|One device|Multiple devices|
|Local storage|Server storage|

### 3. FTP Uses Two Connections

Huge networking concept.

### 4. SMTP Sends Mail

POP3/IMAP receive mail.

---

# Recommended Next Room

Continue with:

[Protocols and Servers 2](https://tryhackme.com/room/protocolsandservers2?utm_source=chatgpt.com)

You’ll learn:

- sniffing attacks
    
- MITM attacks
    
- TLS
    
- SSH
    
- Hydra password attacks