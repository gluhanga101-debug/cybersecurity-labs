# 04 — Data Security and SQL Server Authentication

## Overview
Lab exercise from ICT-4405 Data Security at the University of Denver covering SQL Server authentication modes, login management, and access control. Configured both Windows Authentication and SQL Server Authentication on a SQL Server 2022 instance, created and compared logins with different privilege levels, and documented the differences in authentication method and server role assignment.

## Tools Used
- SQL Server 2022 (SQL Server 17.0.1000.7)
- SQL Server Management Studio (SSMS)
- VMware Horizon — lab environment
- Windows Server (DU domain — DU\Grace.Luhanga)

## Environment
- University of Denver virtual lab accessed via VMware Horizon on macOS
- AdventureWorks2025 sample database
- Domain authentication via DU Active Directory with Duo MFA

## What I Did
1. Documented existing Windows Authentication login (DU\Grace.Luhanga) and its server roles
2. Identified authentication method: DU institutional credentials with Duo MFA enforced
3. Enabled SQL Server and Windows Authentication mode via server properties
4. Restarted the SQL Server instance to apply the authentication mode change
5. Created a new SQL Server login with password authentication and no enforced password policy
6. Connected to the instance using the new login and compared privilege levels

## Key Finding
The Windows-authenticated login (DU\Grace.Luhanga) held the sysadmin server role, granted automatically during installation. The newly created SQL Server login held only the public role, with no ability to manage the server, view other databases, or modify settings. The contrast demonstrates the principle of least privilege and the risk of over-provisioning during initial setup.

## Topics Covered
- Windows Authentication vs SQL Server Authentication
- Server roles and privilege assignment
- Authentication mode configuration and service restart requirements
- Login creation and access control in SSMS
