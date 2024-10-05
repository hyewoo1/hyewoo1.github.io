---
title: "Docker container backup and restore"
categories:
  - Docker
tags:
  - Docker
---

## Docker container backup and restore

### 1. Check the current container ID

```bash
$ docker ps -a
CONTAINER ID   IMAGE                COMMAND                   CREATED         STATUS                            PORTS     NAMES
10ecb9562a8c   myoracle19c:latest   "/bin/sh -c 'exec $O…"   2 minutes ago   Exited (143) About a minute ago             myoracle19c
```

### 2. Save the container as a Docker image
```bash
docker commit 10ecb9562a8c myoracle19c
```

### 3. Save the Docker image to a file
```bash
docker save -o myoracle19c.tar myoracle19c
```

### 4. Load the saved Docker image from the file
```bash
docker load -i myoracle19c.tar
```

### 5. Run the container according to your environment
```bash
docker run --name myoracle19c ^
-p 1521:1521 ^
-e ORACLE_SID=ORCL ^
-e ORACLE_PWD=Oracle123 ^
-e ORACLE_CHARACTERSET=AL32UTF8 ^
-v C:\oracle-19c\oradata:/opt/oracle/oradata ^
myoracle19c:latest
```
