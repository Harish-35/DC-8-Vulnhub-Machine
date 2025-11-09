
# DC-8 — Vulnhub Machine

Welcome to the DC-8 VulnHub machine repository. This repository contains documentation and a walkthrough-friendly README for the DC-8 virtual machine challenge. The goal of this README is to help pentesters and students set up the VM, enumerate, exploit, and escalate privileges responsibly for learning purposes.

> WARNING: Only run this VM in an isolated lab environment. Do not run exploits or scans against systems you do not own or have permission to test.

## Table of Contents
- [Overview](#overview)
- [Objectives](#objectives)
- [Prerequisites](#prerequisites)
- [Getting started (Setup)](#getting-started-setup)
- [Recommended tools](#recommended-tools)
- [Approach & Enumeration (high-level)](#approach--enumeration-high-level)
- [Hints (if you get stuck)](#hints-if-you-get-stuck)
- [What you will find (non-spoiler)](#What-you-will-find-non-spoiler)
- [Mitigations & Fixes](#Mitigations-&-Fixes)
- [Contributing](#Contributing)
- [License](#License)

## Overview
DC-8 is an intentionally vulnerable virtual machine intended for educational purposes. It contains a mix of common misconfigurations and application-level vulnerabilities designed to teach enumeration, web application testing, credential discovery, and privilege escalation techniques.

This README intentionally avoids giving away explicit exploit details or final flags; instead it provides a safe structure to guide learning and capture the box.

## Objectives
- Perform network and service enumeration to discover attack surface.
- Identify and exploit application-level vulnerabilities (web, authentication, file handling, etc.). 
- Obtain an initial foothold (user-level shell).
- Escalate privileges to obtain root-level access.

## Prerequisites
- A host machine capable of running virtualization software (VirtualBox / VMware).
- Basic familiarity with Linux, networking, and pentesting tools.
- Recommended OS: Kali Linux or a pentesting distribution (or equivalent toolset installed).

## Getting started (Setup)

1. Download and import the DC-8 VM image (OVA) into your virtualization software.
   - If you have a compressed image, extract it first.
2. Configure the VM network adapter:
   - Use Bridged Adapter or NAT with port forwarding for safe, isolated testing.
   - If using Bridged Adapter, set your attacking machine to the same Bridged Adapter network.
3. Start the VM and identify its IP:
   - Use your virtualization UI or run `ifconfig` / `ip addr` from inside the VM if you have console access.
4. From your attacker machine, verify reachability:
   - Example: `ping <target-ip>`

VM resource recommendation:
- 1-2 vCPUs
- 1-2 GB RAM
- 8-16 GB disk

## Recommended tools
- Recon & enumeration:
  - nmap
  - netcat (nc)
- Web testing:
  - browser, Burp Suite, OWASP ZAP
  - gobuster, dirb, wfuzz
  - sqlmap
- Post-exploitation:
  - linpeas/linenum, sudo-checks, GTFOBins
  - ssh, scp, socat
- General:
  - john, hashcat (for password cracking)
  - python / php (for quick scripts)

## Approach & Enumeration (high-level)
1. Recon:
   - Run an initial port scan with nmap: `nmap <target-ip>, nmap -A <target-ip> `
   - Identify open ports and services.
2. Service enumeration:
   - For HTTP(S): visit the site, probe directories with gobuster/dirb, look for forms, headers, and interesting endpoints.
   - For SSH: if open, attempt credential discovery after locating potential usernames and passwords from the web app or configuration files.
   - For SMB/FTP/MySQL: enumerate shares and anonymous access.
3. Web application testing:
   - Look for common issues: SQLi, LFI/RFI, file upload flaws, authentication bypasses, insecure direct object references (IDOR), and command injection.
4. Gaining a foothold:
   - Use discovered vulnerabilities or credentials to get a shell. Common methods include reverse shells (nc/ncat), SSH, or web shell uploads.
5. Post-exploitation & privilege escalation:
   - Gather system info: kernel version, installed packages, sudo rights, SUID binaries, running services, and user home directories.
   - Use linpeas/linenum to accelerate discovery; manually verify interesting findings.
   - Test misconfigurations (weak sudo rules, cron jobs, writable scripts) and exploit them to escalate to root.

## Hints (if you get stuck)
- Check web app directories and hidden files with a wordlist — sensitive files (configs, backup files) sometimes contain credentials.
- Pay attention to services that reveal version numbers — they may point to known exploits.
- Search for credentials left in config files, or for hard-coded passwords in scripts.
- Use sudo -l to quickly learn about allowed privileged commands for the current user.

## What you will find (non-spoiler)
- A web application with at least one discoverable vulnerability that leads to privilege escalation.
- Multiple misconfigurations and weak practices intended for learning (e.g., exposed files, default creds, or writable scripts).
- Two capture targets typically present in VulnHub boxes:
  - user.txt (user privilege)
  - root.txt (root privilege)

## Mitigations & Fixes
To harden a system similar to DC-8, consider:
- Keep software up-to-date and apply security patches.
- Remove or restrict unnecessary services and open ports.
- Use strong, unique passwords and avoid hard-coded credentials.
- Limit sudo privileges and follow the principle of least privilege.
- Secure web applications: validate input, sanitize file uploads, and employ proper authentication and session management.
- Monitor logs and use intrusion detection where possible.

## Contributing
If you have corrections, add walkthrough notes (clearly labeled SPOILERS), or improvements for this README, feel free to open a PR or submit an issue. When contributing walkthrough content, please mark spoilers clearly and provide educational context for each step.

## License
This repository is provided for educational purposes. Use at your own risk. No warranties are provided. Do not use any information or code for unauthorized activities.

---

