---
title: "Key IT and Infosec Terms Explained"
date: 2025-03-18
tags: [Cybersecurity, IT Security, Risk Management]
categories: [Information Security]
pinned: false
---

## Vulnerability
A vulnerability refers to a weakness or flaw in an organization's environment, including applications, networks, and infrastructure, that can be exploited by external threats. These vulnerabilities are cataloged in MITRE’s Common Vulnerabilities and Exposures (CVE) database and are assigned a Common Vulnerability Scoring System (CVSS) score to measure their severity. The CVSS score, ranging from 0 to 10, considers factors like the attack vector (e.g., network, local), attack complexity, privilege requirements, user interaction, and the impact on confidentiality, integrity, and availability (CIA) of data.

A fundamental cybersecurity principle is:

**Threat + Vulnerability = Risk**

- **Threat** – A potential event that can cause harm.
- **Vulnerability** – A known weakness that can be exploited.
- **Risk** – The potential consequences of a threat successfully exploiting a vulnerability.

For example, SQL Injection is a vulnerability that allows attackers to manipulate database queries. If an attacker can exploit it remotely over the internet without authentication, it would have a higher CVSS score than one requiring internal network access and authentication.

## Threat
A threat is an event or action that increases the likelihood of an attack, such as a hacker attempting to exploit a vulnerability. Some vulnerabilities pose greater threats than others, depending on how easy they are to exploit and the potential damage they can cause.

For example, a zero-day exploit is a high-risk threat because it targets a vulnerability that has no available fix, making it highly attractive to attackers.

## Exploit
An exploit is a tool, code, or technique that takes advantage of a vulnerability to gain unauthorized access or control over a system. Exploit code is often shared on platforms like Exploit-DB and Rapid7's Vulnerability Database, and it can be found on open-source sites like GitHub.

For example, if a vulnerability in a web application allows remote code execution (RCE), an attacker can develop an exploit to run commands on the compromised server.

## Risk
Risk is the potential for harm or damage when a threat exploits a vulnerability. Risk management involves assessing the likelihood and impact of an exploit to determine the level of risk and prioritize mitigation.

According to ISO 31000, risk is the "effect of uncertainty on objectives" and can be either negative (a threat) or positive (an opportunity).

A useful way to differentiate these terms:

- **Risk** – A negative event that could happen.
- **Threat** – A negative event that is happening.
- **Vulnerability** – A flaw that could lead to a threat.

### Risk Assessment Matrix:

| Likelihood | Impact | Risk Level |
|------------|--------|------------|
| High | High | Critical (5) |
| Medium | High | High (4) |
| Low | High | Medium (3) |
| High | Low | Medium (3) |
| Medium | Medium | Medium (3) |
| Low | Medium | Low (2) |
| Low | Low | Minimal (1) |

For example, if a vulnerability has a high chance of being exploited and could allow unauthorized access to sensitive business data, it would be classified as a high-risk issue requiring immediate remediation.

## Asset Management
To effectively protect an organization’s data and infrastructure, it is crucial to first identify and inventory all assets. This process is a key aspect of defensive security and forms the foundation for vulnerability management.

### Asset Inventory
A comprehensive asset inventory helps an organization track all its IT assets and apply appropriate security controls. This includes:

- **Information Technology (IT) assets** – Computers, servers, network devices, and storage systems.
- **Operational Technology (OT) assets** – Industrial control systems and IoT devices.
- **Software assets** – Operating systems, applications, and cloud services.
- **Development assets** – Code repositories, APIs, and microservices.
- **Mobile assets** – Smartphones, tablets, and mobile applications.
- **Physical assets** – Security cameras, biometric scanners, and hardware devices.

Asset management tools are used to track, classify, and protect these resources.

### Application and System Inventory
Organizations must maintain an accurate and up-to-date inventory of all their data assets to implement proper security measures. This includes:

#### On-Premises Data Storage:
- **Endpoint storage:** Hard drives (HDDs, SSDs) in laptops, desktops, and servers.
- **External storage:** USB drives, SD cards, and network-attached storage (NAS).
- **Legacy storage:** Optical media (CDs, DVDs, Blu-ray), floppy disks, and tape drives.

#### Cloud Storage:
- **Public Cloud Providers:** Amazon Web Services (AWS), Google Cloud Platform (GCP), Microsoft Azure.
- **Multi-Cloud Environments:** Organizations using multiple cloud providers need tools to track assets across different platforms.

#### Software-as-a-Service (SaaS) Applications:
- **Common SaaS tools:** Google Drive, Microsoft OneDrive, Dropbox, Adobe Creative Cloud, Microsoft Office 365.

#### Applications and Systems:
- **Installed software:** Business applications, databases, enterprise resource planning (ERP) systems.
- **Networking infrastructure:** Firewalls, routers, switches, and intrusion detection/prevention systems (IDS/IPS).
- **Security tools:** Data Loss Prevention (DLP) systems and security monitoring solutions.

### The Importance of Comprehensive Asset Management
Every network device, application, and data repository in an organization is a potential attack surface. Without proper tracking, a company cannot secure what it does not know exists. Failing to document assets increases the risk of unpatched vulnerabilities, misconfigurations, and unauthorized access.

Since organizations constantly add and remove assets, they must continuously update their inventory to ensure an accurate and complete security strategy.

