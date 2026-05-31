# 05 — Database Authorization: SQL Server Logins, Users, and Role Assignment

**Course:** ICT-4405 Data Security — University of Denver  
**Week:** 3

## Overview
Explored SQL Server's two-layer access model by creating server-level logins, mapping them to database users inside AdventureWorks2025, and assigning different database roles to demonstrate how authentication and authorization operate independently. Tested access outcomes by executing queries under different role configurations and observing permission-based failures.

## Tools Used
- SQL Server 2022 (SQL Server 17.0.1000.7)
- SQL Server Management Studio (SSMS)
- VMware Horizon — DU lab environment
- AdventureWorks2025 sample database

## Environment
- University of Denver virtual lab accessed via VMware Horizon on macOS
- Domain authentication via DU Active Directory (DU\Grace.Luhanga)

## What I Did
1. Created two SQL Server logins (mynewlogin and mynewlogin2) with password authentication and no enforced password policy
2. Mapped mynewlogin to a database user in AdventureWorks2025 and assigned db_datareader, db_datawriter, and db_ddladmin roles
3. Mapped mynewlogin2 with db_datareader only
4. Executed a CREATE TABLE statement under each login to observe role-based outcomes
5. Documented the permission denied error returned by mynewlogin2 as evidence of least privilege enforcement

## Key Finding
SQL Server enforces a strict two-layer access model: a login authenticates at the instance level but cannot access any database without an explicit user mapping. Role assignments at the database level directly determine what actions are authorized. mynewlogin successfully created a table (db_ddladmin); mynewlogin2 received a permission denied error for the same command (db_datareader only). The contrast illustrated how granular role-based access control works in practice.

## Topics Covered
- SQL Server login vs database user distinction
- db_datareader, db_datawriter, db_ddladmin role privileges
- User Mapping configuration in SSMS
- Authentication vs authorization separation
- Principle of least privilege enforcement
