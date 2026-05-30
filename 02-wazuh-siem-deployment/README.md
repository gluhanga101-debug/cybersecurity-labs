# 02 — Wazuh SIEM Deployment and Agent Integration

## Overview
Continuation of Lab 01. Deployed Wazuh 4.7.5 as a centralised SIEM on an Ubuntu VM, connected Windows Server 2022 as a monitored agent, and confirmed live telemetry and automated compliance scanning. Three installation attempts across two sessions were required due to hardware constraints and leftover file conflicts.

## Tools Used
- Wazuh 4.7.5 — SIEM platform (all-in-one installation)
- Ubuntu VM (log01) — SIEM host
- Windows Server 2022 — monitored endpoint
- Oracle VirtualBox — virtualisation platform
- Firefox / Microsoft Edge — dashboard access

## Environment
- Ubuntu VM allocated 4 GB RAM after increasing from default allocation
- Windows Server VM shut down during Wazuh installation to free host memory
- Agent connected via host-only network (192.168.56.x)

## What I Did
1. Attempted Wazuh installation — failed due to hardware requirements (under 4 GB RAM)
2. Re-attempted with -i flag to bypass hardware check — indexer service failed to start, installation self-cleaned
3. Shut down Windows Server VM, increased Ubuntu VM RAM to 4 GB in VirtualBox settings
4. Ran installer with --overwrite flag to clear leftover files from previous attempt — installation succeeded
5. Accessed Wazuh dashboard at https://192.168.56.2, confirmed API online and all health checks passing
6. Accessed dashboard from within Windows Server VM to bypass cross-VM clipboard limitations
7. Used Deploy new agent wizard to generate and run PowerShell install command on Windows Server
8. Started WazuhSvc and confirmed agent WIN-EEJ1IO99076 active with 100 percent coverage

## Key Finding
Within minutes of the agent connecting, Wazuh automatically ran a CIS Microsoft Windows Server 2022 Benchmark v1.0.0 assessment without any manual configuration. Result: **35 percent compliance** — 120 controls passed, 216 failed, across 342 evaluated controls. This reflects the gap between a default Windows Server installation and hardened production standards.

## Challenges
- Hardware constraints required shutting down one VM to install another — sequencing matters in a limited-RAM environment
- Clipboard sharing between Mac host and VirtualBox VMs was not functional — worked around by accessing Wazuh dashboard from inside the target VM
- Leftover files from failed installation attempt required --overwrite flag on the third run

## Report
[View Lab Continuation Report (PDF)](./Lab_Continuation_Wazuh_and_Agent.pdf)
