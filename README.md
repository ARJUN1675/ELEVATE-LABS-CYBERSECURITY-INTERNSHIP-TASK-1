# Task 1: Scan Your Local Network for Open Ports

Objective: Learn to discover open ports on devices in your local network to understand network exposure. 
Tools:  Nmap (free), Wireshark (optional).

# Nmap Local Network Scan Report

This repository documents the process and findings of scanning a local network using Nmap. The purpose is to identify active hosts, open ports, and to assess potential security risks by following these steps:

1. Install Nmap from the official website.[10]
2. Find your local IP range (e.g., 192.168.1.0/24).
3. Run the command below to perform a TCP SYN scan:
   ```
   nmap -sS 192.168.1.0/24
   ```
4. Note down the IP addresses and open ports found.
5. Optionally, analyze a packet capture of the scan using Wireshark.
6. Research common services running on those open ports.
7. Identify any potential security risks from the open ports detected.
8. Save the scan results as a text or HTML file for documentation.

These steps help in understanding the network layout, identifying accessible services, and recognizing possible security vulnerabilities that could exist due to open ports. The results and relevant analysis files are provided within this repository for reference and learning.

-----

# My NMAP TCP SYN scan result:
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
5357/tcp open  wsdapi

---

# Common services running on those open ports.

# 1. 135/tcp — Microsoft RPC (msrpc)**
* **Service:** Microsoft Remote Procedure Call (MS RPC).
* **Use:** Provides a way for Windows services and applications to communicate across a network.
* **Details:** Often used for DCOM (Distributed Component Object Model), service control, and remote management functions. Attackers sometimes target it for remote code execution vulnerabilities.

# 2. 139/tcp — NetBIOS Session Service (netbios-ssn)**
* **Service:** NetBIOS Session Service.
* **Use:** Supports file and printer sharing over older Windows networks. Used by SMB (Server Message Block) over NetBIOS.
* **Details:** This port is mainly used in legacy Windows networking (Windows 95/98/NT/2000). It allows applications to establish a session to exchange data. Modern networks often disable it in favor of port 445.

# 3. 445/tcp — Microsoft-DS (microsoft-ds)**
* **Service:** Microsoft Directory Services (SMB over TCP).
* **Use:** Supports direct SMB (Server Message Block) file sharing, printer sharing, and remote Windows administration.
* **Details:** A critical port for Windows networking, domain controllers, and Active Directory. This port is a frequent target of malware and exploits (e.g., WannaCry, EternalBlue).

# 4. 5357/tcp — Web Services on Devices API (wsdapi)
* **Service:** Web Services for Devices API.
* **Use:** Enables discovery and interaction with network-connected devices (like printers and scanners) using SOAP-based web services.
* **Details:** Common in Windows environments where devices auto-discover each other. Generally low-risk but not usually needed on public-facing systems.

# Inshort:
* 135, 139, 445 → Core Windows networking & sharing services (RPC, NetBIOS, SMB).
* 5357 → Device discovery (used for printers/scanners).

-----

# Potential security risks from open ports.

# 1. 135/tcp (MSRPC)
* **Remote Code Execution (RCE):** Attackers can exploit RPC vulnerabilities to execute code remotely.
* **Service Enumeration:** Exposes details about running Windows services, which attackers can use for targeted exploits.
* **Worm Propagation:** Has been abused by worms like **Blaster** in the past.

# 2. 139/tcp (NetBIOS-SSN)
* **Information Leakage:** Attackers can enumerate system names, shares, and users.
* **Unauthorized Access:** Could allow access to shared files/folders if misconfigured.
* **Legacy Vulnerabilities:** Since NetBIOS is old, it’s less secure and often targeted for brute-force or relay attacks.

# 3. 445/tcp (SMB over TCP)
* **Major Exploit Target:** Used in **EternalBlue**, **WannaCry**, and **NotPetya** ransomware.
* **Remote Code Execution & Lateral Movement:** Attackers can compromise one machine and spread through the network.
* **Sensitive Data Exposure:** Allows attackers to access file shares, Active Directory, or credential caches.
* **Brute-force Attacks:** Password guessing on SMB shares.

# 4. 5357/tcp (WSDAPI – Web Services for Devices)
* **Reconnaissance Risk:** Exposes printers, scanners, and devices for enumeration.
* **Unauthorized Device Access:** Misconfigured WSD could allow attackers to interact with devices.
* **Attack Surface Expansion:** Even if not widely exploited, any unnecessary service increases risk.

# Overall Risk:
* 135, 139, 445 → **High risk** if exposed externally. They are frequently abused for **RCE, ransomware, and lateral movement**.
* 5357 → **Medium risk**. Less commonly exploited but unnecessary exposure may allow device-based attacks.

# In short:
1. Biggest risks: Ransomware, unauthorized file access, lateral movement in Windows networks.
2. Root cause: These ports expose **Windows networking services** that attackers love to target.

---

***
