# Hadoop Troubleshooting Guide

## Introduction

Troubleshooting is a critical skill for Hadoop administrators and Data Engineers.

In real-world environments, issues such as NameNode failures, DataNode crashes, Safe Mode problems, and permission errors are common.

This guide provides systematic approaches to identifying and resolving Hadoop issues.

---

# Troubleshooting Methodology

Before fixing any issue:

## Step 1: Identify the Problem

Ask:

* What failed?
* When did it fail?
* Is it a storage issue?
* Is it a network issue?
* Is it a permission issue?

---

## Step 2: Check Running Services

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

Missing service indicates the source of the problem.

---

## Step 3: Check Cluster Health

```bash
hdfs dfsadmin -report
```

Verify:

* Live DataNodes
* Dead DataNodes
* Disk Usage
* Capacity

---

# Problem 1: NameNode Not Starting

## Symptoms

```text
Connection refused

Cannot connect to NameNode
```

---

## Check

```bash
jps
```

NameNode missing.

---

## Possible Causes

* Metadata corruption
* Disk full
* Configuration errors
* Java issues

---

## Troubleshooting

Check logs:

```bash
cd $HADOOP_HOME/logs
```

```bash
cat hadoop-*-namenode-*.log
```

---

## Solution

Verify:

```bash
df -h
```

Ensure sufficient disk space.

Restart:

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

## Check

```bash
jps
```

DataNode missing.

---

## Common Causes

* Network issues
* Disk issues
* Version mismatch
* Storage directory problems

---

## Solution

Check logs:

```bash
cat hadoop-*-datanode-*.log
```

Restart:

```bash
hadoop-daemon.sh start datanode
```

---

# Problem 3: NameNode Safe Mode

## Symptoms

Cannot upload files.

Error:

```text
Name node is in safe mode.
```

---

## Check Status

```bash
hdfs dfsadmin -safemode get
```

---

## Leave Safe Mode

```bash
hdfs dfsadmin -safemode leave
```

---

## Why Safe Mode Happens

* Cluster startup
* Missing blocks
* Insufficient replicas

---

# Problem 4: Permission Denied

## Error

```text
Permission denied
```

---

## Check Permissions

```bash
hdfs dfs -ls /data
```

---

## Fix Permissions

```bash
hdfs dfs -chmod 755 /data
```

---

## Fix Ownership

```bash
hdfs dfs -chown user:group /data
```

---

# Problem 5: Under Replicated Blocks

## Symptoms

```bash
hdfs fsck /
```

Output:

```text
Under replicated blocks
```

---

## Causes

* DataNode failure
* Network issues
* Insufficient nodes

---

## Check

```bash
hdfs dfsadmin -report
```

---

## Solution

Restore failed DataNodes.

HDFS will automatically re-replicate blocks.

---

# Problem 6: Disk Full

## Symptoms

Cannot write files.

Upload failures occur.

---

## Check Disk Usage

```bash
df -h
```

---

## Check HDFS Usage

```bash
hdfs dfs -df -h
```

---

## Solution

* Delete unnecessary files
* Increase storage
* Archive old data

---

# Problem 7: Missing Blocks

## Symptoms

Corrupted files.

---

## Check

```bash
hdfs fsck /
```

---

## Example Output

```text
Missing blocks
```

---

## Solution

Recover from replicas or backup.

---

# Problem 8: Network Connectivity Issues

## Symptoms

Nodes cannot communicate.

Jobs fail unexpectedly.

---

## Check Connectivity

```bash
ping hostname
```

---

## Check Ports

```bash
netstat -tulpn
```

---

## Common Hadoop Ports

| Service            | Port |
| ------------------ | ---- |
| NameNode UI        | 9870 |
| ResourceManager UI | 8088 |
| DataNode UI        | 9864 |

---

# Problem 9: Hadoop Services Not Running

## Check

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

## Verify

```bash
jps
```

---

# Problem 10: Slow Hadoop Performance

## Causes

* Small Files Problem
* Network bottlenecks
* Insufficient memory
* Data skew
* High replication factor

---

## Checks

Disk:

```bash
df -h
```

Memory:

```bash
free -m
```

CPU:

```bash
top
```

---

# Important Log Locations

## Hadoop Logs

```bash
$HADOOP_HOME/logs
```

---

## Common Logs

```text
NameNode Logs

DataNode Logs

ResourceManager Logs

NodeManager Logs
```

---

# Real-World Scenario

## Scenario

A DataNode crashes unexpectedly.

---

## Investigation

Check:

```bash
jps
```

DataNode missing.

---

Check cluster:

```bash
hdfs dfsadmin -report
```

Node appears under dead nodes.

---

## Resolution

Restart DataNode.

```bash
hadoop-daemon.sh start datanode
```

Verify:

```bash
hdfs dfsadmin -report
```

---

# Hadoop Troubleshooting Checklist

## Service Check

```bash
jps
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

## Disk Usage

```bash
df -h
```

---

## HDFS Usage

```bash
hdfs dfs -df -h
```

---

## File System Check

```bash
hdfs fsck /
```

---

# Interview Questions

## Beginner

1. How do you check Hadoop services?
2. How do you check cluster health?

## Intermediate

3. What causes Safe Mode?
4. How do you troubleshoot a dead DataNode?

## Advanced

5. Explain under-replicated blocks.
6. How would you troubleshoot slow Hadoop performance?
7. How do you diagnose NameNode startup failures?

---

# Key Takeaways

* Always start troubleshooting with `jps`.
* Check cluster health using `hdfs dfsadmin -report`.
* Review logs before making changes.
* Safe Mode is normal during startup but can indicate issues.
* HDFS replication provides protection against failures.
* Monitoring is essential for maintaining a healthy cluster.
