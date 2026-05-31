# 07 — Dynamic Data Masking

**Course:** ICT-4405 Data Security — University of Denver  
**Week:** 5

## Overview
Configured Dynamic Data Masking (DDM) on the AdventureWorks2025 database to restrict PII visibility based on user privilege level. Created a test user without a login, applied masking rules to specific columns, and used the EXECUTE AS pattern to compare masked and unmasked query results within the same session without creating separate connections.

## Tools Used
- SQL Server 2022
- SQL Server Management Studio (SSMS)
- VMware Horizon — DU lab environment
- AdventureWorks2025 sample database

## Environment
- University of Denver virtual lab (DU\Grace.Luhanga — sysadmin, bypasses masking by default)
- MaskingTestUser created WITHOUT LOGIN for impersonation testing

## What I Did
1. Created MaskingTestUser WITHOUT LOGIN and granted SELECT on the dbo schema
2. Applied DDM masking functions to sensitive columns in the database
3. Queried as DU\Grace.Luhanga (sysadmin) to confirm full unmasked data returned
4. Used EXECUTE AS USER to impersonate MaskingTestUser and confirmed masked output: FirstName showed as Jxxxn, Bxxxxxa
5. Used REVERT to return session context to the privileged account and confirmed unmasked data visible again

## Key Finding
DDM is a presentation-layer control, not a storage-layer security boundary. Privileged accounts bypass masking entirely, and users with UNMASK permission or direct file access see unmasked values regardless of DDM configuration. This means DDM must always be paired with proper role-based access controls rather than treated as a standalone data protection mechanism. The EXECUTE AS pattern allows mask testing without creating additional logins, which is efficient in a shared lab environment.

## Topics Covered
- Dynamic Data Masking vs static data redaction
- Masking function types and column-level application
- EXECUTE AS USER and REVERT session context switching
- Privilege bypass behavior for sysadmin accounts
- DDM limitations and correct use as a complement to RBAC
- Use cases: customer support dashboards, developer environments, multi-tier applications
