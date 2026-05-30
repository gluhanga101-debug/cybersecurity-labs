# 01 — Credential Attack Simulation and Log-Based Detection

## Overview
Simulated a brute-force credential attack against a Windows Server 2022 Active Directory environment and validated detection using native Windows Security logs. Based on the Udemy course "Active Directory Lab with Nmap and Hydra" recommended by a colleague, adapted for a VirtualBox environment running on macOS.

## Tools Used
- Kali Linux — attacker machine
- Windows Server 2022 — target system
- Active Directory Domain Services (lab.local)
- Nmap 7.99 — network and service reconnaissance
- Hydra v9.6 — brute-force simulation
- Windows Event Viewer — log-based detection

## Environment
- Oracle VirtualBox on macOS
- Host-only network between Kali (attacker) and Windows Server (target)
- Domain user account (student1) provisioned in Active Directory as credential target

## What I Did
1. Confirmed network connectivity between attacker and target using ICMP ping
2. Ran an Nmap service version scan identifying five open ports including SMB (445) and NetBIOS (139)
3. Executed Hydra brute-force against RDP (port 3389) and SMB (port 445)
4. Both protocols blocked at the connection level — RDP returned freerdp failures, SMB returned an invalid reply
5. Filtered Windows Security log for Event ID 4625 and found 13 failed logon entries within a six-minute window matching the attack timeframe

## Key Finding
Detection occurred independently of whether the attack connected. Event ID 4625 captured the attack timeline, targeted account, logon type, and failure reason without any SIEM in place. Temporal clustering of multiple failures per second confirmed automated rather than human activity.

## Challenges
- Hydra's RDP module could not bypass Network Level Authentication (NLA), requiring a pivot to SMB
- SMB returned a protocol-level error rather than authentication failures, but authentication telemetry still appeared in Event Viewer
- VirtualBox networking on macOS required manual adapter configuration not covered in the course

## Report
[View Full Lab Report (PDF)](./Lab_Credential_Attack_Detection.pdf)
