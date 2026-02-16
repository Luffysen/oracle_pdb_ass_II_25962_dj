# Oracle Pluggable Database Assignment II

## Table of Contents

1.  [Introduction](#introduction)
2.  [Task 1: Create Main Pluggable Database](#task-1-create-main-pluggable-database)
3.  [Task 2: Create and Delete Temporary PDB](#task-2-create-and-delete-temporary-pdb)
4.  [Task 3: Oracle Enterprise Manager Express](#task-3-oracle-enterprise-manager-express)
5.  [Challenges Faced & Solutions](#challenges-faced--solutions)
6.  [Integrity Statement](#integrity-statement)
7.  [Submission Details](#submission-details)
8.  [Screenshot Index](#screenshot-index)
9.  [Quick Reference: Essential Commands](#quick-reference-essential-commands)
10. [Contact Information](#contact-information)
11. [How to Use This File](#how-to-use-this-file)
12. [Screenshot Checklist](#screenshot-checklist)

## Introduction

This document details the steps and verification for the Oracle Pluggable Database Assignment II, covering the creation and management of Pluggable Databases (PDBs) and the configuration of Oracle Enterprise Manager (EM) Express.

### Oracle Environment

## Task 1: Create Main Pluggable Database

### Objective

Create a permanent Pluggable Database (PDB) using the exact naming convention: `dj_pdb_25962` with admin user `djamaladine_plsqlauca_25962`.

### Naming Convention Applied

| Component      | Format                                  | Applied Value                 |
| :------------- | :-------------------------------------- | :---------------------------- |
| PDB Name       | `FirstTwoLettersOfFirstName_pdb_StudentID` | `dj_pdb_25962`                |
| Admin Username | `FirstName_plsqlauca_StudentID`         | `djamaladine_plsqlauca_25962` |

### Commands Executed

**Step 1: Connect as SYSDBA**

```bash
sqlplus / as sysdba
```

**Step 2: Verify CDB Status**

```sql
SHOW PDBS;
```

**Initial Output:**

```
    CON_ID CON_NAME                       OPEN MODE  RESTRICTED
---------- ------------------------------ ---------- ----------
         2 PDB$SEED                       READ ONLY  NO
         3 ORCLPDB                        READ WRITE NO
```

**Step 3: Create Pluggable Database**

```sql
CREATE PLUGGABLE DATABASE dj_pdb_25962 
ADMIN USER djamaladine_plsqlauca_25962 IDENTIFIED BY 6860 
CREATE_FILE_DEST='C:\ORACLE21C\ORADATA\ORCL\';
```

<img width="588" height="113" alt="Screenshot 2026-02-16 180125" src="https://github.com/user-attachments/assets/0a81ba96-1040-459b-86cd-2e458eba7908" />


**Step 4: Open the PDB**

```sql
ALTER PLUGGABLE DATABASE dj_pdb_25962 OPEN;
```

<img width="371" height="70" alt="Screenshot 2026-02-16 180850" src="https://github.com/user-attachments/assets/5b923f81-b076-4733-a84c-74056cb7eac8" />


**Step 5: Set Auto-Open (Persist State)**

```sql
ALTER PLUGGABLE DATABASE dj_pdb_25962 SAVE STATE;
```

<img width="395" height="49" alt="image" src="https://github.com/user-attachments/assets/d9b39d31-d208-4cd5-a20c-fc9848ce5b1d" />


**Step 6: Verify PDB Status**

```sql
SHOW PDBS;
```

**Final Output:**
<img width="418" height="91" alt="image" src="https://github.com/user-attachments/assets/c424fe74-ed32-4ed6-ba9e-ba635f4b88bd" />


**Step 7: Verify User Creation**

```sql
ALTER SESSION SET CONTAINER = dj_pdb_25962;
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') FROM DUAL;
```

<img width="531" height="79" alt="image" src="https://github.com/user-attachments/assets/8b080ae0-a7b4-4cb3-8f70-8b40ef77864f" />


```sql
SELECT USERNAME, CREATED, ACCOUNT_STATUS 
FROM DBA_USERS 
WHERE USERNAME = 'DJAMALADINE_PLSQLAUCA_25962';
```

**Output:**
<img width="665" height="104" alt="image" src="https://github.com/user-attachments/assets/06cc6809-37e7-4491-a0b6-7c150498d25a" />


**Step 8: Test User Connection**

```bash
sqlplus djamaladine_plsqlauca_25962/6860@localhost:1521/dj_pdb_25962
```

<img width="514" height="260" alt="image" src="https://github.com/user-attachments/assets/cbc37780-a630-4666-a6f0-06ba4ff5e49b" />

## Task 2: Create and Delete Temporary PDB

### Objective

Demonstrate complete PDB lifecycle management by creating and then deleting a temporary PDB named `dj_to_delete_pdb_25962`.

### Naming Convention Applied

| Component     | Format                                        | Applied Value             |
| :------------ | :-------------------------------------------- | :------------------------ |
| Temp PDB Name | `FirstTwoLettersOfFirstName_to_delete_pdb_StudentID` | `dj_to_delete_pdb_25962` |

### Commands Executed

**Step 1: Ensure in CDB Root**

```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') FROM DUAL;
```
<img width="497" height="65" alt="image" src="https://github.com/user-attachments/assets/9e456e11-82b3-4275-8ea2-4cab43b4f2c5" />


**Step 2: Create Temporary PDB**

```sql
CREATE PLUGGABLE DATABASE dj_to_delete_pdb_25962 
ADMIN USER djamaladine_admin IDENTIFIED BY 6860 
CREATE_FILE_DEST='C:\ORACLE21C\ORADATA\ORCL\';
```

<img width="665" height="80" alt="image" src="https://github.com/user-attachments/assets/7716f1f0-fdb3-4fd9-99cd-06bbb3163017" />


**Step 3: Verify Creation**

```sql
SHOW PDBS;
```

**Output:**

<img width="641" height="86" alt="image" src="https://github.com/user-attachments/assets/76a21b99-5214-4bc2-87fe-2e2d313c5409" />



**Step 4: Close Temporary PDB**

```sql
ALTER PLUGGABLE DATABASE dj_to_delete_pdb_25962 CLOSE IMMEDIATE;
```

<img width="411" height="83" alt="image" src="https://github.com/user-attachments/assets/bb11d8bf-d619-4afb-9337-d61bdbe78d50" />


**Step 5: Drop Temporary PDB**

```sql
DROP PLUGGABLE DATABASE dj_to_delete_pdb_25962 INCLUDING DATAFILES;
```

<img width="616" height="80" alt="image" src="https://github.com/user-attachments/assets/471b2a3b-70f3-4677-9676-ce7aefac6bc2" />

**Step 6: Confirm Deletion**

```sql
SHOW PDBS;
```

**Final Output:**

<img width="653" height="134" alt="image" src="https://github.com/user-attachments/assets/f2832173-f3bf-4878-8a29-cb47d8e85c6e" />


## Task 3: Oracle Enterprise Manager Express

### Objective

Configure and access Oracle Enterprise Manager (EM) Express to verify PDB status through GUI.

### EM Express Configuration

**Check Current Status**

```sql
SELECT DBMS_XDB_CONFIG.GETHTTPSPORT() FROM DUAL;
```

<img width="646" height="72" alt="image" src="https://github.com/user-attachments/assets/9774c973-99e1-4f85-93cb-a5350f6f38f6" />


**Enable Global Port for PDB Access**

```sql
EXEC DBMS_XDB_CONFIG.SETGLOBALPORTENABLED(TRUE);
```

<img width="486" height="64" alt="image" src="https://github.com/user-attachments/assets/584608ed-021e-464c-aa4b-379a3dc02d22" />


**Listener Verification**

```bash
lsnrctl status
```

**Key Output:**

<img width="960" height="464" alt="image" src="https://github.com/user-attachments/assets/9e120ba7-1ce4-4708-a818-3393f62418c2" />


### Access EM Express

**URL:** `https://localhost:5500/em`
<img width="821" height="325" alt="image" src="https://github.com/user-attachments/assets/96b7f2cb-89a0-4118-b78d-8a16892b35f5" />


**Verified Elements:**

*   ✅ Oracle environment details displayed
*   ✅ `dj_pdb_25962` listed as active pluggable database
*   ✅ `dj_to_delete_pdb_25962` NOT listed (confirmed deletion)
*   ✅ Service status shows `READY`


## Challenges Faced & Solutions

| # | Challenge                   | Error Code                                  | Root Cause                                      | Solution Applied                                        |
| :- | :-------------------------- | :------------------------------------------ | :---------------------------------------------- | :------------------------------------------------------ |
| 1 | Missing file specification  | `ORA-65016: FILE_NAME_CONVERT must be specified` | No file destination provided                    | Used `CREATE_FILE_DEST` clause with OMF                 |
| 2 | Operation not allowed       | `ORA-65040: operation not allowed from within a pluggable database` | Attempted to create PDB while connected to a PDB | Switched to `CDB$ROOT` using `ALTER SESSION SET CONTAINER = CDB$ROOT` |
| 3 | Syntax errors               | `ORA-00922: missing or invalid option`      | Missing spaces between SQL keywords             | Added proper spacing between clauses                    |
| 4 | PDB already closed          | `ORA-65020: pluggable database already closed` | Attempted to close a PDB in `MOUNTED` state     | Proceeded directly to `DROP` command                    |
| 5 | Invalid file path format    | `ORA-65005: missing or invalid file name pattern` | Used Linux-style paths on Windows               | Used Windows paths with `C:\ORACLE21C\...`            |

### Lessons Learned

*   Always verify container context before creating or dropping PDBs.
*   Use Oracle Managed Files (OMF) with `CREATE_FILE_DEST` to avoid path specification issues.
*   Maintain proper SQL syntax with spaces between keywords.
*   Check listener status to confirm EM Express accessibility.
*   Document each step with screenshots for verification.

## Integrity Statement

I, Djamaladine (Student ID: 25962), hereby confirm that:

*   ✅ This assignment was completed individually without collaboration
*   ✅ All SQL commands were manually typed and executed by me
*   ✅ No commands, screenshots, or repositories were copied from classmates
*   ✅ No AI tools (ChatGPT, Claude, etc.) were used to generate SQL commands or solutions
*   ✅ All screenshots reflect my actual execution environment and database instance
*   ✅ The work presented is my own original work





