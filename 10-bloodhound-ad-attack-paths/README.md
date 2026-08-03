# 10 — Active Directory Attack Path Analysis with BloodHound CE

**Type:** Independent Study / Collaborative Lab  


## Overview
Built a hand-crafted Active Directory attack chain from scratch in an isolated VirtualBox lab, then used BloodHound Community Edition to collect, ingest, and visualise the path. The goal was to understand exactly which misconfigurations produce which edges in the graph — working from first principles rather than seeding a pre-built vulnerable domain. Completed as a first penetration testing engagement alongside a SOC colleague.

## Tools Used
- Kali Linux 2025.4 — attacker machine
- Windows Server 2022 — Domain Controller (lab.local)
- VirtualBox — host-only isolated network
- BloodHound Community Edition (Docker Compose)
- SharpHound v2.13.0 — Active Directory collector
- Neo4j — graph database backend
- PowerShell / dsacls — misconfiguration injection

## Environment
- Fully isolated host-only network (no external connectivity)
- Domain: lab.local
- Three VMs: Kali Linux attacker, Windows Server 2022 Domain Controller
- BloodHound CE deployed via Docker on Kali

## What I Did

### 1. Lab Setup
- Installed and configured Active Directory Domain Services on Windows Server 2022
- Created an Organizational Unit (BH_Lab) and populated it with users and groups
- Deployed BloodHound CE via Docker Compose and confirmed UI accessible at localhost:8080

### 2. Hand-Crafted Attack Chain
Built a three-hop privilege escalation path using only legitimate AD features, no exploits:

**Users and groups created:**
- `analyst01` — low-privilege domain user (attacker foothold)
- `it.helpdesk` — service account
- `Helpdesk Team` — security group

**Misconfigurations injected:**
1. Granted `analyst01` the ForceChangePassword right over `it.helpdesk` via dsacls (ACL edge)
2. Added `it.helpdesk` as a member of `Helpdesk Team`
3. Nested `Helpdesk Team` into `Domain Admins`

**Resulting attack path:**
```
analyst01 → ForceChangePassword → it.helpdesk → MemberOf → Helpdesk Team → MemberOf → Domain Admins
```

Validated the path using `Get-ADGroupMember "Domain Admins" -Recursive` before running any BloodHound collection.

### 3. SharpHound Collection and Ingestion
- Ran SharpHound with `-c All` collection method to capture ACL data required for the ForceChangePassword edge
- Ingested the resulting ZIP into BloodHound CE
- Confirmed the graph matched the hand-crafted chain exactly using the shortest path to Domain Admins query

## Key Finding
BloodHound does not exploit anything. Every hop in the attack path was a legitimate, intended Active Directory feature. The path from a powerless user to Domain Admin was built entirely from permissions that pile up quietly over time through normal administrative activity. A list-based access review would not surface it. Graph-based analysis does.

The manual chain-building approach (rather than using a pre-seeded lab) meant that each edge in the graph had a known, traceable cause: a specific dsacls command, a specific group membership, a specific nesting decision.

## Challenges
- Neo4j ingestion errors during initial BloodHound CE setup (JSONDecodeError / 0 Files) required Docker troubleshooting and container log review to resolve
- SharpHound collection requires domain-joined context and the All collection method for ACL edges to appear — default collection misses the ForceChangePassword edge
- VirtualBox host-only networking required careful adapter configuration to maintain isolation while allowing Kali to reach the domain controller

## What This Maps To
- MITRE ATT&CK: T1069 (Permission Groups Discovery), T1484 (Domain Policy Modification), T1078 (Valid Accounts)
- Real-world relevance: BloodHound paths exist in most production AD environments due to years of accumulated permission drift — the same tool used offensively here is directly applicable to blue-team remediation work
