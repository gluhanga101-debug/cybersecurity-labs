# 09 — Database Auditing: SQL Server Audit and Login Event Tracking

**Course:** ICT-4405 Data Security — University of Denmark  
**Week:** 8

## Overview
Configured SQL Server auditing at both server and database levels to capture authentication events, privilege changes, schema modifications, and data access activity. Validated audit output by generating login events including a deliberate failed login, and reviewed the Log File Viewer to confirm event capture and understand audit event ordering.

## Tools Used
- SQL Server 2022
- SQL Server Management Studio (SSMS)
- VMware Horizon — DU lab environment
- Windows 11 (Version 23H2) — Horizon VM OS

## What I Did
1. Reviewed Windows Update history to document OS patch status (most recent cumulative update applied 8/21/2025)
2. Enabled login auditing in SQL Server Properties and restarted the SQL Server service to apply the change
3. Created a server-level SQL Server Audit object targeting the Windows Security event log
4. Created a database-level audit specification referencing the server audit, targeting specific tables and principals
5. Generated authentication events including a successful login (DU\Grace.Luhanga) and a deliberate failed login attempt (fakeuser — error 18456, Severity 14)
6. Reviewed Log File Viewer to confirm both events captured, noting reverse chronological display order

## Key Finding
SQL Server auditing operates in two layers that must be configured in the correct sequence: the server-level audit must exist and be enabled before the database-level specification can reference it. Enabling login auditing in Server Properties does not take effect until the SQL Server service is fully restarted — reopening SSMS is not sufficient. The four audit event categories (authentication, privilege changes, schema changes, data access) together satisfy audit trail requirements for SOC 2, HIPAA, and GDPR compliance frameworks.

## Topics Covered
- SQL Server Audit architecture: server audit vs database audit specification
- Four database audit event categories and their compliance relevance
- Login auditing configuration and service restart requirement
- Error 18456 failed login events and severity levels
- Log File Viewer navigation and reverse chronological ordering
- Patching trade-offs in shared database environments
- SOC 2, HIPAA, GDPR, PCI-DSS audit trail requirements
