# Hadoop Commands

## Introduction

Hadoop provides command-line utilities to interact with HDFS and manage the Hadoop cluster.

These commands are used for:

* File Management
* Directory Management
* Data Upload/Download
* Cluster Monitoring
* Administration
* Troubleshooting

---

# HDFS Command Structure

```bash
hdfs dfs <command>
```

Example:

```bash
hdfs dfs -ls /
```

---

# File and Directory Commands

## List Files

### Command

```bash
hdfs dfs -ls /
```

### Purpose

Lists files and directories.

### Example Output

```text
drwxr-xr-x   - hdfs supergroup          0 2025-01-01 /user
```

### Common Mistake

```bash
hdfs dfs ls /
```

❌ Missing '-'

Correct:

```bash
hdfs dfs -ls /
```

---

## Create Directory

### Command

```bash
hdfs dfs -mkdir /data
```

### Create Nested Directories

```bash
hdfs dfs -mkdir -p /project/raw/data
```

### Purpose

Creates directories in HDFS.

---

## Upload File

### Command

```bash
hdfs dfs -put employee.csv /data
```

### Alternative

```bash
hdfs dfs -copyFromLocal employee.csv /data
```

### Purpose

Copies local file to HDFS.

---

## Download File

### Command

```bash
hdfs dfs -get /data/employee.csv .
```

### Alternative

```bash
hdfs dfs -copyToLocal /data/employee.csv .
```

### Purpose

Copies file from HDFS to local system.

---

## View File Content

### Command

```bash
hdfs dfs -cat /data/employee.csv
```

### Purpose

Displays file contents.

---

## View First Few Lines

### Command

```bash
hdfs dfs -head /data/employee.csv
```

### Purpose

Displays beginning of file.

---

## View Last Few Lines

### Command

```bash
hdfs dfs -tail /data/employee.csv
```

### Purpose

Displays last part of file.

---

## Remove File

### Command

```bash
hdfs dfs -rm /data/employee.csv
```

### Force Delete

```bash
hdfs dfs -rm -f /data/employee.csv
```

---

## Remove Directory

### Command

```bash
hdfs dfs -rmdir /data
```

### Delete Non-Empty Directory

```bash
hdfs dfs -rm -r /data
```

---

# File Information Commands

## Check Disk Usage

### Command

```bash
hdfs dfs -du -h /data
```

### Purpose

Shows file sizes.

---

## Check Total Space Used

### Command

```bash
hdfs dfs -df -h
```

### Purpose

Shows HDFS capacity and usage.

---

## Count Files and Directories

### Command

```bash
hdfs dfs -count /data
```

### Example Output

```text
DIR_COUNT FILE_COUNT CONTENT_SIZE
```

---

# Permissions Commands

## View Permissions

```bash
hdfs dfs -ls /data
```

---

## Change Permissions

```bash
hdfs dfs -chmod 755 /data/file.txt
```

---

## Change Owner

```bash
hdfs dfs -chown hdfs:user /data/file.txt
```

---

## Change Group

```bash
hdfs dfs -chgrp data_team /data/file.txt
```

---

# Cluster Administration Commands

## Check HDFS Report

```bash
hdfs dfsadmin -report
```

### Purpose

Displays:

* Cluster Capacity
* Used Space
* Remaining Space
* Live DataNodes
* Dead DataNodes

### Interview Question

How do you check cluster health?

Answer:

```bash
hdfs dfsadmin -report
```

---

## Safe Mode Status

### Check Safe Mode

```bash
hdfs dfsadmin -safemode get
```

### Enter Safe Mode

```bash
hdfs dfsadmin -safemode enter
```

### Leave Safe Mode

```bash
hdfs dfsadmin -safemode leave
```

---

## List Blocks

```bash
hdfs fsck /data/file.csv -files -blocks
```

### Purpose

Shows:

* Block Information
* Block Locations
* Replication Details

---

# Monitoring Commands

## Check Running Daemons

```bash
jps
```

### Example Output

```text
NameNode
DataNode
ResourceManager
NodeManager
```

### Purpose

Shows Java processes running on Hadoop node.

---

## View HDFS Filesystem

```bash
hdfs dfs -ls -R /
```

### Purpose

Recursively lists all files.

---

# Useful Linux Commands for Hadoop

## Check Disk Usage

```bash
df -h
```

---

## Check Memory

```bash
free -m
```

---

## Check CPU

```bash
top
```

---

## Check Open Ports

```bash
netstat -tulpn
```

---

# Real-World Scenarios

## Scenario 1

Upload a dataset.

```bash
hdfs dfs -mkdir /startup_funding

hdfs dfs -put startup.csv /startup_funding
```

Verify:

```bash
hdfs dfs -ls /startup_funding
```

---

## Scenario 2

Check DataNode Health

```bash
hdfs dfsadmin -report
```

Verify:

* Live Nodes
* Dead Nodes
* Capacity

---

## Scenario 3

Investigate Missing Blocks

```bash
hdfs fsck /
```

---

# Common Mistakes

## Mistake 1

```bash
hdfs dfs ls
```

Wrong syntax.

Correct:

```bash
hdfs dfs -ls
```

---

## Mistake 2

Uploading file without creating directory.

```bash
hdfs dfs -put file.csv /project
```

Directory may not exist.

Create first:

```bash
hdfs dfs -mkdir /project
```

---

## Mistake 3

Confusing Local and HDFS paths.

Local:

```bash
/home/cloudera/file.csv
```

HDFS:

```bash
/project/file.csv
```

---

# Frequently Asked Interview Questions

1. How do you upload a file to HDFS?
2. Difference between put and copyFromLocal?
3. How do you check cluster health?
4. What is Safe Mode?
5. How do you verify block locations?
6. How do you check disk usage in HDFS?
7. What command shows Hadoop daemons?

---

# Quick Cheat Sheet

```bash
hdfs dfs -ls /

hdfs dfs -mkdir /data

hdfs dfs -put file.csv /data

hdfs dfs -cat /data/file.csv

hdfs dfs -rm /data/file.csv

hdfs dfsadmin -report

hdfs dfsadmin -safemode get

hdfs fsck /

jps
```
