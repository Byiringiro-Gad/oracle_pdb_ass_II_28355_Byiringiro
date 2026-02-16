## Byiringiro Gad               28355  

## Course: Database Development with PL/SQL (INSY 8311)
## Instructor: Eric Maniraguha  
  
## Individual Assignment II – ORACLE PLUGGABLE DATABASE MANAGEMENT

Repository Link: https://github.com/Byiringiro-Gad/oracle_pdb_ass_II_28355_Byiringiro    
PDB Name Created: by_pdb_28355  
Issues Encountered: No  


# 1. Assignment Objective

This assignment helped me to have hands on skills for Oracle Multitenant Architecture using Oracle Database 21c.

I can 

- Create and manage a Pluggable Database (PDB)
- Create and configure a local user inside a PDB
- Verify PDB operational state
- create and drop a database
- Configure and access Oracle Enterprise Manager (OEM)



# 2. Oracle Environment Used

| Component | Specification |
|-----------|--------------|
| Database Version | Oracle Database 21c Enterprise Edition |
| Architecture | Multitenant (CDB + PDB) |
| Tools Used | SQL*Plus, Oracle Enterprise Manager (EM Express) |
| Operating System | Windows |



# 3. TASK 1 – Create a New Pluggable Database

## Step 1 – Connect as SYSDBA

```sql
sqlplus / as sysdba
```

---

## Step 2 – Created the PDB

```sql
CREATE PLUGGABLE DATABASE by_pdb_28355
ADMIN USER byiringiro_plsqlauca_28355 IDENTIFIED BY auca123
FILE_NAME_CONVERT = ('pdbseed','by_pdb_28355');
```

---

## Step 3 – Open the PDB

```sql
ALTER PLUGGABLE DATABASE by_pdb_28355 OPEN;
```

---

## Step 4 – Save the PDB State

```sql
ALTER PLUGGABLE DATABASE by_pdb_28355 SAVE STATE;
```

---

## Step 5 – Verify PDB Creation

```sql
SHOW PDBS;
```

### PDB Successfully Created

![PDB Creation Evidence](screenshots/PDB%20creation/by_pdbs(creation).png)


## Step 6 – Verify Open Mode

```sql
SELECT name, open_mode FROM v$pdbs;
```

### PDB Open Mode (READ WRITE)

![PDB Creation Evidence](screenshots/PDB%20creation/by_pdbs(openstate1).png)

![PDB Open Mode Evidence](screenshots/PDB%20creation/by_pdbs(open_state).png)

Verification confirms:
- Correct PDB name
- Open mode READ WRITE
- Operational database


# 4. User Configuration Inside the PDB

## Switch to the Created PDB

```sql
ALTER SESSION SET CONTAINER = by_pdb_28355;
```

## Unlock and Configure User

```sql
ALTER USER byiringiro_plsqlauca_28355 ACCOUNT UNLOCK;
ALTER USER byiringiro_plsqlauca_28355 DEFAULT TABLESPACE USERS;
ALTER USER byiringiro_plsqlauca_28355 QUOTA UNLIMITED ON USERS;
GRANT CONNECT, RESOURCE, CREATE SESSION TO byiringiro_plsqlauca_28355;
```


# 5. TASK 2 – Create and Delete a Temporary PDB

## Step 1 – Switch to Root Container

```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
```

## Step 2 – Create Temporary PDB

```sql
CREATE PLUGGABLE DATABASE By_to_delete_pdb_28355
ADMIN USER temp IDENTIFIED BY auca123
FILE_NAME_CONVERT = ('pdbseed','By_to_delete_pdb_28355');

ALTER PLUGGABLE DATABASE by_to_delete_pdb_28355 OPEN;
```

### Verify Temporary PDB Exists

```sql
SHOW PDBS;
```

### Temporary PDB Created

![Temporary PDB Created](screenshots/PDB%20deletion/by_to_delete(creation).png)


## Step 3 – Delete Temporary PDB Completely

```sql
ALTER PLUGGABLE DATABASE by_to_delete_pdb_28355 CLOSE IMMEDIATE;
DROP PLUGGABLE DATABASE by_to_delete_pdb_28355 INCLUDING DATAFILES;
```

### Confirm Deletion

```sql
SHOW PDBS;
```

### Temporary PDB Deleted

![Temporary PDB Deleted](screenshots/PDB%20deletion/by_to_delete(delection).png)

Verification confirms complete removal.

---

# 6. TASK 3 – Oracle Enterprise Manager (OEM)

Oracle Enterprise Manager Database Express was configured and accessed via HTTPS.

Login performed using SYSDBA credentials.

Dashboard confirms:
- Active Container Database
- Registered PDB: `By_pdb_28355`
- Administrative session active
- Username visible
- Successful configuration

OEM Dashboard(loged in as sys)

![OEM Dashboard Evidence](screenshots/OEM%20dashboard/OEM_HOME(logged_in_as_sys).png)

OEM Dashboard(loged in as by_pdb_28355)  
![OEM Dashboard Evidence](screenshots/OEM%20dashboard/OEM_HOME(byiringiro_pdbs_28355).png)

---

# 7. Challenges Encountered and Solutions

| Issue | Cause | Resolution |
|-------|--------|------------|
| OEM login failure | Incorrect container selection | Selected correct container and port |
| Invalid credentials | User not unlocked | Account unlocked and password reset |
| Listener connection issue | Service configuration mismatch | Verified using `lsnrctl status` |



# Conclusion

After this assignment I understand:

- Oracle Multitenant Architecture
- PDB creation and configuration
- Local user management
- PDB lifecycle control
- Enterprise Manager monitoring

All required tasks were completed and verified through SQL output and OEM dashboard evidence.

# 9. Academic Integrity Statement

I confirm that this assignment was completed individually in my own Oracle environment.

All commands were executed by me.
All screenshots reflect my personal work.
No collaboration, impersonation, or reuse of screenshots occurred.

I understand that violation of academic integrity policies results in zero marks.
