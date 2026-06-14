# HDFS Commands

## Introduction

HDFS commands are used to interact with the Hadoop Distributed File System.

They allow users to:

* Create directories
* Upload files
* Download files
* Manage permissions
* Monitor cluster health
* Troubleshoot issues

Most commands follow this syntax:

```bash
hdfs dfs <command>
```

---

# File and Directory Commands

## List Files

```bash
hdfs dfs -ls /
```

Example:

```bash
hdfs dfs -ls /user/hive
```

Purpose:

Display files and directories.

---

## Recursive Listing

```bash
hdfs dfs -ls -R /
```

Lists all files recursively.

---

## Create Directory

```bash
hdfs dfs -mkdir /project
```

---

## Create Nested Directories

```bash
hdfs dfs -mkdir -p /data/raw/2025
```

---

## Remove Empty Directory

```bash
hdfs dfs -rmdir /project
```

---

## Delete File

```bash
hdfs dfs -rm /project/data.csv
```

---

## Delete Directory Recursively

```bash
hdfs dfs -rm -r /project
```

---

## Force Delete

```bash
hdfs dfs -rm -r -f /project
```

---

# File Upload Commands

## Upload File

```bash
hdfs dfs -put sales.csv /data
```

---

## Alternative Upload Command

```bash
hdfs dfs -copyFromLocal sales.csv /data
```

---

## Upload Directory

```bash
hdfs dfs -put logs/ /data
```

---

# File Download Commands

## Download File

```bash
hdfs dfs -get /data/sales.csv .
```

---

## Alternative Download

```bash
hdfs dfs -copyToLocal /data/sales.csv .
```

---

## Download Directory

```bash
hdfs dfs -get /data/logs .
```

---

# File Viewing Commands

## Display File Content

```bash
hdfs dfs -cat /data/file.csv
```

---

## Display First Lines

```bash
hdfs dfs -head /data/file.csv
```

---

## Display Last Lines

```bash
hdfs dfs -tail /data/file.csv
```

---

## Display File in Text Format

```bash
hdfs dfs -text /data/file.gz
```

Useful for compressed files.

---

# File Information Commands

## Check File Size

```bash
hdfs dfs -du -h /data
```

---

## Check HDFS Capacity

```bash
hdfs dfs -df -h
```

---

## Count Files and Directories

```bash
hdfs dfs -count /data
```

Output:

```text
DIR_COUNT FILE_COUNT CONTENT_SIZE
```

---

# Copy and Move Commands

## Copy File Inside HDFS

```bash
hdfs dfs -cp /data/file.csv /backup
```

---

## Move File

```bash
hdfs dfs -mv /data/file.csv /archive
```

---

## Rename File

```bash
hdfs dfs -mv old.csv new.csv
```

---

# Permission Commands

## View Permissions

```bash
hdfs dfs -ls /data
```

---

## Change Permission

```bash
hdfs dfs -chmod 755 /data
```

---

## Recursive Permission Change

```bash
hdfs dfs -chmod -R 755 /data
```

---

## Change Owner

```bash
hdfs dfs -chown hdfs:user /data
```

---

## Change Group

```bash
hdfs dfs -chgrp analytics /data
```

---

# Replication Commands

## View Replication

```bash
hdfs dfs -ls /data
```

---

## Set Replication Factor

```bash
hdfs dfs -setrep 3 /data/file.csv
```

---

## Recursive Replication

```bash
hdfs dfs -setrep -R 3 /data
```

---

# Cluster Administration Commands

## Cluster Report

```bash
hdfs dfsadmin -report
```

Shows:

* Capacity
* Usage
* Live DataNodes
* Dead DataNodes

---

## Refresh Nodes

```bash
hdfs dfsadmin -refreshNodes
```

---

## Finalize Upgrade

```bash
hdfs dfsadmin -finalizeUpgrade
```

---

# Safe Mode Commands

## Check Safe Mode

```bash
hdfs dfsadmin -safemode get
```

---

## Enter Safe Mode

```bash
hdfs dfsadmin -safemode enter
```

---

## Leave Safe Mode

```bash
hdfs dfsadmin -safemode leave
```

---

# FSCK Commands

FSCK = File System Check

---

## Check Entire HDFS

```bash
hdfs fsck /
```

---

## Check Specific File

```bash
hdfs fsck /data/file.csv
```

---

## Check Blocks

```bash
hdfs fsck /data/file.csv -files -blocks
```

---

## Check Locations

```bash
hdfs fsck /data/file.csv -locations
```

---

# Snapshot Commands

## Create Snapshot

```bash
hdfs dfs -createSnapshot /data snap1
```

---

## Delete Snapshot

```bash
hdfs dfs -deleteSnapshot /data snap1
```

---

# Quota Commands

## Set Namespace Quota

```bash
hdfs dfsadmin -setQuota 1000 /data
```

---

## Remove Namespace Quota

```bash
hdfs dfsadmin -clrQuota /data
```

---

## Set Space Quota

```bash
hdfs dfsadmin -setSpaceQuota 100g /data
```

---

## Remove Space Quota

```bash
hdfs dfsadmin -clrSpaceQuota /data
```

---

# Monitoring Commands

## Check Running Hadoop Processes

```bash
jps
```

Example:

```text
NameNode
DataNode
ResourceManager
NodeManager
```

---

## Check Disk Usage

```bash
df -h
```

---

## Check Memory Usage

```bash
free -m
```

---

## Monitor Processes

```bash
top
```

---

# HDFS Balancer Commands

## Run Balancer

```bash
hdfs balancer
```

Purpose:

Balance data across DataNodes.

---

## Run with Threshold

```bash
hdfs balancer -threshold 5
```

---

# Useful Administrative Commands

## Format NameNode

⚠️ Dangerous

```bash
hdfs namenode -format
```

Used only during initial setup.

---

## Start DFS

```bash
start-dfs.sh
```

---

## Stop DFS

```bash
stop-dfs.sh
```

---

## Start YARN

```bash
start-yarn.sh
```

---

## Stop YARN

```bash
stop-yarn.sh
```

---

# Real-World Scenarios

## Scenario 1: Upload Dataset

```bash
hdfs dfs -mkdir /datasets

hdfs dfs -put sales.csv /datasets
```

Verify:

```bash
hdfs dfs -ls /datasets
```

---

## Scenario 2: Check Cluster Health

```bash
hdfs dfsadmin -report
```

Verify:

* Live Nodes
* Dead Nodes
* Capacity

---

## Scenario 3: Investigate Corruption

```bash
hdfs fsck /
```

---

## Scenario 4: Change Replication

```bash
hdfs dfs -setrep 5 /data/file.csv
```

---

# Commands Frequently Asked in Interviews

## List Files

```bash
hdfs dfs -ls
```

---

## Upload File

```bash
hdfs dfs -put
```

---

## Download File

```bash
hdfs dfs -get
```

---

## Cluster Health

```bash
hdfs dfsadmin -report
```

---

## Safe Mode

```bash
hdfs dfsadmin -safemode get
```

---

## File System Check

```bash
hdfs fsck /
```

---

## Replication

```bash
hdfs dfs -setrep
```

---

# Common Mistakes

## Mistake 1

```bash
hdfs dfs ls /
```

Wrong

Correct:

```bash
hdfs dfs -ls /
```

---

## Mistake 2

Using local paths with HDFS commands.

Wrong:

```bash
hdfs dfs -cat /home/cloudera/file.csv
```

---

Correct:

```bash
hdfs dfs -cat /data/file.csv
```

---

## Mistake 3

Formatting NameNode accidentally.

```bash
hdfs namenode -format
```

Never run on a production cluster.

---

# Quick Command Cheat Sheet

```bash
hdfs dfs -ls /

hdfs dfs -mkdir /data

hdfs dfs -put file.csv /data

hdfs dfs -get /data/file.csv .

hdfs dfs -cat /data/file.csv

hdfs dfs -rm /data/file.csv

hdfs dfs -du -h /data

hdfs dfs -df -h

hdfs dfsadmin -report

hdfs dfsadmin -safemode get

hdfs fsck /

hdfs dfs -setrep 3 file.csv

jps
```

---

# Key Takeaways

* `hdfs dfs` commands are used for daily HDFS operations.
* `dfsadmin` commands are used for administration.
* `fsck` is used for health checks and troubleshooting.
* Safe Mode commands are frequently asked in interviews.
* `jps` is often the first command used during troubleshooting.
* Understanding HDFS commands is essential for Hadoop administration and Data Engineering roles.
