# 06 — Database Encryption: Transparent Data Encryption (TDE)

**Course:** ICT-4405 Data Security — University of Denver  
**Week:** 4

## Overview
Implemented Transparent Data Encryption on the AdventureWorks2025 database using a three-layer key hierarchy: database master key, server certificate, and database encryption key. Verified encryption state through system views and documented the correct sequence for both enabling and removing TDE.

## Tools Used
- SQL Server 2022
- SQL Server Management Studio (SSMS)
- VMware Horizon — DU lab environment
- AdventureWorks2025 sample database

## Environment
- University of Denver shared Horizon VM
- Existing master key from a prior session was present, requiring diagnostic queries before proceeding

## What I Did
1. Queried sys.symmetric_keys to check for an existing master key before creating one
2. Created the Database Master Key encrypted by AES_256
3. Created server certificate MyServerCert protected by the master key
4. Created a Database Encryption Key for AdventureWorks2025 using AES_256 encrypted by MyServerCert
5. Enabled encryption with ALTER DATABASE and confirmed encryption_state via sys.dm_database_encryption_keys
6. Documented the correct reversal sequence: disable encryption, drop DEK, drop certificate, drop master key

## Key Finding
TDE follows a strict three-layer hierarchy and each layer must exist before the next can be created. Working on a shared Horizon VM meant the master key and encryption settings from a prior session were already in place, causing both the CREATE MASTER KEY and SET ENCRYPTION ON commands to return errors. Running verification queries first would have immediately revealed the existing state. The lab reinforced that error messages in SQL Server accurately describe the environment rather than indicating something broken.

## Topics Covered
- Transparent Data Encryption architecture and use cases
- Database master key, server certificate, and DEK hierarchy
- sys.dm_database_encryption_keys and encryption state codes
- Regulatory compliance context: HIPAA, PCI-DSS, SOX
- TDE reversal sequence
- Shared environment considerations (VMware Horizon)
