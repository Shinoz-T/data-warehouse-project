# Setup Instructions

## 1. Create the `dvdrental` Database

a. **Open PowerShell:**

     ```powershell
     psql -U postgres
Enter the password for the postgres user when prompted.

b. **Create the dvdrental Database:**
CREATE DATABASE dvdrental;

c. **Verify Database Creation:**
\l

d. **Exit psql:**
exit

## 2. Restore the Sample Database from a Tar File

### Download and Extract the dvdrental.tar File

Download the `dvdrental.tar` file and extract it to a directory, e.g., `C:\sampledb\postgres\dvdrental.tar`.

### Load the Data
    ```powershell
    pg_restore -U postgres -d dvdrental C:\sampledb\postgres\dvdrental.tar
Enter the password for the postgres user when prompted.

## 3. Verify the Sample Database

### Connect to the PostgreSQL Server
    ```powershell
    psql -U postgres

Switch to the dvdrental Database
\c dvdrental

Display All Tables
\dt






