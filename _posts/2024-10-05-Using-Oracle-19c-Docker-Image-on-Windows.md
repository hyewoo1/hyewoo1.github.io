---
title: "Using Oracle 19c Docker Image on Windows"
categories:
  - Database
tags:
  - OracleDB
  - Docker
  - Oracle19c
  - Database
---

## Using Oracle 19c Docker Image on Windows

This guide will help you use the Oracle 19c Docker image on Windows. To run Docker containers on Windows, you need to use **Docker Desktop**. All steps are explained to work within the Windows environment without using Linux commands.

### 1. **Install Docker Desktop**

1. Download and install **[Docker Desktop](https://www.docker.com/products/docker-desktop)**.
2. After installation, launch Docker Desktop.
3. You can configure Docker Desktop to use the **Windows Subsystem for Linux 2 (WSL 2)** backend for better performance.
4. Verify that Docker is running correctly by executing the following command in Command Prompt or PowerShell:

```bash
docker --version
```

### 2. **Pull the Image**

Download the custom image provided on Docker Hub to your local machine. Open Command Prompt or PowerShell and enter the following command:

```bash
docker pull doctorkirk/oracle-19c
```

This command pulls the Oracle 19c image to your local machine.

### 3. **Create a Local Data Directory**

Create a local directory where the Oracle container will store its data. You can do this using File Explorer or a command.

1. **Using Windows File Explorer**, create a directory at your desired path.
  Example: `C:\oracle-19c\oradata`

Or, **using Command Prompt**, create the directory with the following command:

```bash
mkdir C:\oracle-19c\oradata
```

### 4. **Set Directory Permissions**

Unlike Linux, you don't need to set permissions using the `chown` command on Windows. The Docker container will use this directory without any permission issues, so you can skip this step.

### 5. **Run the Container**

Now, run the container with the following command in Command Prompt or PowerShell:

```bash
docker run --name oracle-19c ^
-p 1521:1521 ^
-e ORACLE_SID=ORCL ^
-e ORACLE_PWD=Oracle123 ^
-e ORACLE_CHARACTERSET=AL32UTF8 ^
-v C:\oracle-19c\oradata:/opt/oracle/oradata ^
doctorkirk/oracle-19c
```

#### Explanation:
- `--name oracle-19c`: Names the container `oracle-19c`
- `-p 1521:1521`: Maps local port 1521 to container port 1521 (Oracle Listener)
- `-e ORACLE_SID=ORCL`: Sets the Oracle SID to `ORCL` (modifiable)
- `-e ORACLE_PWD=Oracle123`: Sets the password for Oracle SYS, SYSTEM accounts to `Oracle123` (modifiable)
- `-e ORACLE_CHARACTERSET=AL32UTF8`: Sets the database character set to UTF-8
- `-v C:\oracle-19c\oradata:/opt/oracle/oradata`: Links the local directory to the container directory for data volume

### 6. **Connect to Oracle Database**

Once the container is running, you can connect to the database using SQL*Plus or Oracle SQL Developer. To use SQL*Plus, execute the following command in Command Prompt:

```bash
sqlplus sys/Oracle123@//localhost:1521/ORCL as sysdba
```

You are now ready to use the custom Oracle 19c Docker image on Windows.

### Summary
1. Install **Docker Desktop**
2. Download the image with `docker pull doctorkirk/oracle-19c`
3. Create a local directory for data storage
4. Run the container with the Docker run command
5. Connect to the database using SQL*Plus or Oracle SQL Developer

Follow these steps to easily use Oracle 19c in a Docker container on Windows.
