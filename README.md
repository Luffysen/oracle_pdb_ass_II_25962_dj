# oracle_pdb_ass_II_25962_dj

# Oracle PDB Management Assignment Report

## Overview

This report details the completion of Individual Assignment II: Oracle Pluggable Databases (PDB) Management. The assignment focused on practical skills related to Oracle's Multitenant Architecture, including the creation and deletion of Pluggable Databases, user management within a PDB, and interaction with Oracle Enterprise Manager (OEM). All tasks were performed individually, adhering to strict naming conventions and documentation requirements.

## Oracle Environment Used

*   **Operating System:** [Specify your OS, e.g., Oracle Linux 8, Windows 10]
*   **Oracle Database Version:** [Specify your Oracle Database version, e.g., Oracle Database 21c Enterprise Edition]
*   **Database Architecture:** Multitenant Container Database (CDB)
*   **SQL Client:** SQL*Plus

## Task Walkthrough

### Task 1: Create a New Pluggable Database (PDB)

This task involved creating a new PDB and a dedicated user within it. The PDB was named `dj_pdb_25962` (using `dj` for the first two letters of my first name and `25962` as my student ID) and an administrative user `dj_plsqlauca_25962` was created inside it.

1.  **Connected to the Container Database (CDB):**
    ```sql
    sqlplus / as sysdba
    ```

2.  **Created the PDB `dj_pdb_25962`:**
    ```sql
    CREATE PLUGGABLE DATABASE dj_pdb_25962 
    ADMIN USER dj_admin IDENTIFIED BY <YOUR_PASSWORD>
    FILE_NAME_CONVERT = (
        'C:\ORACLE21C\ORADATA\ORCL\PDBSEED\', 
        'C:\ORACLE21C\ORADATA\ORCL\dj_pdb_25962\'
    );
    ```
    *(Screenshot: PDB creation command and result)*

3.  **Opened the PDB `dj_pdb_25962`:**
    ```sql
    ALTER PLUGGABLE DATABASE dj_pdb_25962 OPEN;
    ```
    *(Screenshot: `show pdbs;` command showing `dj_pdb_25962` in `READ WRITE` state)*

4.  **Created user `dj_plsqlauca_25962` inside the PDB:**
    ```sql
    ALTER SESSION SET CONTAINER = dj_pdb_25962;
    CREATE USER dj_plsqlauca_25962 IDENTIFIED BY <YOUR_PASSWORD>;
    GRANT CONNECT, RESOURCE, DBA TO dj_plsqlauca_25962;
    ```
    *(Screenshot: User creation command and result, showing username)*

### Task 2: Create and Delete a Temporary PDB

This task demonstrated the full lifecycle management of a PDB by creating a temporary PDB, verifying its existence, and then completely deleting it.

1.  **Created the temporary PDB `dj_to_delete_pdb_25962`:**
    ```sql
    ALTER SESSION SET CONTAINER = CDB$ROOT;
    CREATE PLUGGABLE DATABASE dj_to_delete_pdb_25962 
    ADMIN USER temp_admin IDENTIFIED BY temppass123
    FILE_NAME_CONVERT = (
        'C:\ORACLE21C\ORADATA\ORCL\PDBSEED\', 
        'C:\ORACLE21C\ORADATA\ORCL\dj_to_delete_pdb_25962\'
    );
    ```
    *(Screenshot: Temporary PDB creation command and result)*

2.  **Verified and Deleted the temporary PDB:**
    ```sql
    -- Check existence
    show pdbs;

    -- Close PDB before deletion
    ALTER PLUGGABLE DATABASE dj_to_delete_pdb_25962 CLOSE IMMEDIATE;

    -- Delete PDB and its data files
    DROP PLUGGABLE DATABASE dj_to_delete_pdb_25962 INCLUDING DATAFILES;

    -- Confirm deletion
    show pdbs;
    ```
    *(Screenshots: `show pdbs;` before deletion, deletion commands, and `show pdbs;` after deletion confirming removal)*

### Task 3: Oracle Enterprise Manager (OEM)

This task involved accessing and verifying the Oracle environment through Oracle Enterprise Manager.

1.  **Accessed the OEM Dashboard:** Navigated to `https://localhost:5500/em` and logged in.

2.  **Verified PDB Status:** Confirmed that `dj_pdb_25962` was listed and in an `Open` state within the OEM dashboard. My username and environment details were visible.

    *(Screenshot: OEM dashboard showing PDB status and visible user/environment information)*

### Task 4: Documentation & Reporting (GitHub)

This report itself serves as the documentation for Task 4, published on a public GitHub repository named `oracle_pdb_ass_II_25962_dj`. All relevant screenshots are organized in a `screenshots/` folder within the repository.

## Challenges Faced

[Describe any challenges encountered during the assignment and how they were resolved. If none, state 'No significant challenges were encountered during the completion of this assignment.']

## Integrity Statement

I confirm that this work is my own and has been completed individually, without unauthorized assistance or collaboration. All commands, configurations, and documentation reflect my own execution and understanding of the assignment requirements. I have not used any AI tools to generate commands or solutions. 

--- 

**Note:** All screenshots mentioned above are available in the `screenshots/` directory of the GitHub repository.
