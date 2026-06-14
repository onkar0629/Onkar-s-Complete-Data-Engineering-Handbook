# HDFS Troubleshooting Guide

## Introduction

Troubleshooting is one of the most important skills for a Data Engineer or Hadoop Administrator.

In production environments, failures are normal:

* DataNodes fail
* NameNodes crash
* Disks fill up
* Network issues occur
* Blocks become corrupt

The goal is not to prevent all failures.

The goal is to:

```text
Detect

Diagnose

Recover
```

quickly and safely.

---

# Troubleshooting Workflow

Always follow this sequence:

```text
1. Identify Problem

↓

2. Check Services

↓

3. Check Logs

↓

4. Check Cluster Health

↓

5. Verify Storage

↓

6. Fix Root Cause

↓

7. Validate Recovery
```

---

# Step 1: Check Running Services

First command:

```bash
jps
```

Expected Output:

```text
NameNode

DataNode

ResourceManager

NodeManager
```

Missing process indicates problem area.

---

# Step 2: Check Cluster Health

```bash
hdfs dfsadmin -report
```

Verify:

* Live Nodes
* Dead Nodes
* Capacity
* Remaining Space

---

# Step 3: Check HDFS Health

```bash
hdfs fsck /
```

Checks:

* Missing Blocks
* Corrupt Blocks
* Under-Replicated Blocks

---

# Problem 1: NameNode Not Starting

## Symptoms

```text
Connection Refused

Cannot Access HDFS

NameNode Missing In JPS
```

---

## Verify

```bash
jps
```

Output:

```text
DataNode
```

No NameNode present.

---

## Check Logs

```bash
cd $HADOOP_HOME/logs
```

```bash
tail -100 hadoop-*-namenode-*.log
```

---

## Common Causes

### Disk Full

```bash
df -h
```

---

### Corrupted Metadata

Issues with:

```text
FsImage

Edit Logs
```

---

### Configuration Errors

Check:

```bash
hdfs getconf -confKey fs.defaultFS
```

---

## Solution

Fix underlying issue and restart:

```bash
start-dfs.sh
```

---

# Problem 2: DataNode Not Starting

## Symptoms

```text
Dead DataNode
```

shown in:

```bash
hdfs dfsadmin -report
```

---

## Verify

```bash
jps
```

Missing:

```text
DataNode
```

---

## Check Logs

```bash
tail -100 hadoop-*-datanode-*.log
```

---

## Common Causes

### Disk Failure

```bash
df -h
```

---

### Permission Issues

Verify storage directory.

---

### Network Issues

Check connectivity.

---

## Start DataNode

```bash
hadoop-daemon.sh start datanode
```

---

# Problem 3: Safe Mode Stuck

## Symptoms

Upload Fails:

```bash
hdfs dfs -put file.csv /data
```

Error:

```text
Name node is in safe mode
```

---

## Verify

```bash
hdfs dfsadmin -safemode get
```

---

## Causes

* Missing blocks
* Cluster startup
* Insufficient replicas

---

## Leave Safe Mode

```bash
hdfs dfsadmin -safemode leave
```

---

# Problem 4: Under-Replicated Blocks

## Check

```bash
hdfs fsck /
```

Example:

```text
Under replicated blocks
```

---

## Causes

* Dead DataNode
* Network failure
* Disk failure

---

## Verify Cluster

```bash
hdfs dfsadmin -report
```

---

## Solution

Restore failed DataNode.

Allow HDFS to re-replicate blocks.

---

# Problem 5: Corrupt Blocks

## Symptoms

```bash
hdfs fsck /
```

Output:

```text
Corrupt Blocks
```

---

## Causes

* Disk corruption
* Hardware issues
* Incomplete writes

---

## Verify

```bash
hdfs fsck /data/file.csv -files -blocks
```

---

## Solution

Use healthy replicas.

HDFS usually recovers automatically.

---

# Problem 6: Missing Blocks

## Symptoms

```text
Data Loss Warning
```

---

## Verify

```bash
hdfs fsck /
```

---

## Causes

* All replicas lost
* Multiple node failures

---

## Solution

Restore from backup.

Missing blocks cannot be recreated automatically.

---

# Problem 7: Disk Full

## Symptoms

```text
Write Failure

No Space Left On Device
```

---

## Verify

```bash
df -h
```

---

## Check HDFS Usage

```bash
hdfs dfs -df -h
```

---

## Solutions

### Delete Old Data

```bash
hdfs dfs -rm -r /archive
```

---

### Add New DataNodes

Increase storage capacity.

---

### Compress Data

Use:

* Snappy
* Gzip
* ORC
* Parquet

---

# Problem 8: Permission Denied

## Error

```text
Permission Denied
```

---

## Check Permissions

```bash
hdfs dfs -ls /data
```

---

## Fix Permission

```bash
hdfs dfs -chmod 755 /data
```

---

## Fix Ownership

```bash
hdfs dfs -chown user:group /data
```

---

# Problem 9: Network Failure

## Symptoms

```text
Heartbeat Lost

Node Unreachable
```

---

## Verify Connectivity

```bash
ping hostname
```

---

## Check Ports

```bash
netstat -tulpn
```

---

## Important Ports

| Service         | Port |
| --------------- | ---- |
| NameNode UI     | 9870 |
| DataNode UI     | 9864 |
| ResourceManager | 8088 |

---

# Problem 10: Cluster Imbalance

## Symptoms

```text
DN1 = 95% Used

DN2 = 20% Used
```

---

## Check

```bash
hdfs dfsadmin -report
```

---

## Solution

Run:

```bash
hdfs balancer
```

---

# Problem 11: NameNode Memory Issues

## Symptoms

```text
Slow Metadata Operations

Frequent GC

OutOfMemoryError
```

---

## Causes

Usually:

```text
Small Files Problem
```

---

## Verify

Monitor memory usage:

```bash
free -m
```

---

## Solution

* Merge small files
* Increase RAM
* Use Federation

---

# Problem 12: Hadoop Services Not Running

## Verify

```bash
jps
```

---

## Start HDFS

```bash
start-dfs.sh
```

---

## Start YARN

```bash
start-yarn.sh
```

---

## Verify Again

```bash
jps
```

---

# Important Log Locations

## Hadoop Logs

```bash
$HADOOP_HOME/logs
```

---

## NameNode Logs

```text
hadoop-*-namenode-*.log
```

---

## DataNode Logs

```text
hadoop-*-datanode-*.log
```

---

## ResourceManager Logs

```text
hadoop-*-resourcemanager-*.log
```

---

# Real Production Incident

## Scenario

Cluster:

```text
20 DataNodes
```

Suddenly:

```text
3 DataNodes Dead
```

---

## Investigation

```bash
hdfs dfsadmin -report
```

Shows:

```text
Dead Nodes = 3
```

---

## Check

```bash
ping datanode-host
```

---

## Root Cause

Network switch failure.

---

## Resolution

Switch repaired.

Nodes restarted.

Replication restored automatically.

---

# Interview Scenarios

## Scenario 1

NameNode not visible in JPS.

Question:

First step?

Answer:

Check NameNode logs.

---

## Scenario 2

Safe Mode persists after startup.

Question:

What command checks status?

Answer:

```bash
hdfs dfsadmin -safemode get
```

---

## Scenario 3

Cluster storage reaches 100%.

Question:

What should you do?

Answer:

* Delete old data
* Add nodes
* Compress datasets

---

# Most Useful Troubleshooting Commands

```bash
jps
```

Check Hadoop services.

---

```bash
hdfs dfsadmin -report
```

Check cluster health.

---

```bash
hdfs fsck /
```

Check filesystem health.

---

```bash
df -h
```

Check disk usage.

---

```bash
free -m
```

Check memory.

---

```bash
hdfs dfsadmin -safemode get
```

Check Safe Mode.

---

```bash
hdfs balancer
```

Balance cluster.

---

# Quick Revision

### Service Check

```bash
jps
```

### Cluster Health

```bash
hdfs dfsadmin -report
```

### Filesystem Health

```bash
hdfs fsck /
```

### Disk Usage

```bash
df -h
```

### Memory Usage

```bash
free -m
```

### Safe Mode

```bash
hdfs dfsadmin -safemode get
```

### Balancer

```bash
hdfs balancer
```

---

# Key Takeaways

* Always begin troubleshooting with `jps`.
* Check logs before making changes.
* Use `dfsadmin -report` to assess cluster health.
* Use `fsck` to identify block-related problems.
* Most production issues involve storage, replication, permissions, or network failures.
* Understanding troubleshooting workflows is highly valuable for interviews and real-world operations.
