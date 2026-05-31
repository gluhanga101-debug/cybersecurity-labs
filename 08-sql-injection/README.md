# 08 — SQL Injection Attacks and Parameterized Query Defence

**Course:** ICT-4405 Data Security — University of Denver  
**Week:** 7

## Overview
Demonstrated a SQL injection attack against a vulnerable T-SQL query by injecting OR 1=1 into a WHERE clause, then implemented and validated the parameterized query defence using sp_executesql. Compared results between the vulnerable and secure implementations to confirm the defence held.

## Tools Used
- SQL Server 2022
- SQL Server Management Studio (SSMS)
- VMware Horizon — DU lab environment
- AdventureWorks2025 sample database (Person.Person table)

## What I Did
1. Ran a legitimate query with a valid LastName input — 103 Smith rows returned as expected
2. Injected OR 1=1 into the vulnerable concatenated query — 19,972 rows returned (full table dump)
3. Verified the injected SQL via the SSMS Messages tab: WHERE LastName = '' OR '1' = '1'
4. Implemented sp_executesql with typed parameters to fix the vulnerability
5. Re-ran the injection string against the parameterized version — zero rows returned, confirming the defence held

## Key Finding
SQL injection requires no specialized tooling and produces immediate results. A single injected string returned the entire Person.Person table (19,972 rows) from what appeared to be a scoped search. Parameterized queries fixed this completely because SQL Server treats the input as a data literal bound at execution time rather than parsing it as SQL logic. The SSMS Messages tab was critical for making the attack mechanism visible — Results showed what was returned but Messages showed what actually executed.

## Topics Covered
- SQL injection mechanics: concatenation-based WHERE clause manipulation
- OR 1=1 payload construction and T-SQL single-quote escaping
- sp_executesql parameterized query implementation
- SSMS Messages tab for query execution verification
- Defence-in-depth: input validation, stored procedures, least-privilege accounts, WAF rules
- OWASP Top 10 context (A03:2021 Injection)
