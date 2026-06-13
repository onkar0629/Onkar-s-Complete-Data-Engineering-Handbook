# DataNode

## Introduction

A DataNode is the worker node in HDFS responsible for storing actual data blocks.

While the NameNode manages metadata, DataNodes store and retrieve the real data requested by users and applications.

Every Hadoop cluster contains one NameNode and multiple DataNodes.

Without DataNodes, HDFS would have metadata but no actual data storage.

---

# What is a DataNode?

A DataNode is a worker daemon in HDFS that stores data blocks and serves read/write requests from clients.

### Responsibilities

* Store data blocks
* Read data blocks
* Write data blocks
* Send Heartbeats
* Send Block Reports
* Participate in replication

---

# DataNode Architecture

```text
                    NameNode
                       |
      ---------------------------------
      |               |               |
      v               v               v

   DataNode1      DataNode2      DataNode3

   Block A        Block B        Block C
```

DataNodes store the actual file blocks assigned by the NameNode.

---

# Role of DataNode

## Data Storage

Stores actual HDFS blocks.

Example:

Suppose file:

```text
employee.csv
```

Size:

```text
300 MB
```

Block Size:

```text
128 MB
```

HDFS creates:

```text
Block 1 = 128 MB

Block 2 = 128 MB

Block 3 = 44 MB
```

These blocks are stored across DataNodes.

---

## Read Requests

When a client reads a file:

1. NameNode provides block locations.
2. Client contacts DataNode.
3. DataNode sends requested block.

---

## Write Requests

When a file is uploaded:

1. NameNode selects DataNodes.
2. Client sends blocks.
3. DataNodes store blocks.

---

# DataNode Storage Structure

Actual blocks are stored on local disks.

Example:

```text
/data/hdfs/dn/current
```

Contains:

```text
Block Files

Metadata Files
```

---

# Heartbeats

## What is a Heartbeat?

A signal sent from DataNode to NameNode.

Purpose:

```text
"I am alive"
```

---

## Frequency

Approximately:

```text
3 Seconds
```

---

## Why Important?

Allows NameNode to monitor cluster health.

---

# What Happens If Heartbeat Stops?

NameNode assumes:

```text
DataNode Failed
```

Actions:

1. Mark node as dead.
2. Identify missing replicas.
3. Start re-replication.

---

# Block Reports

## What is a Block Report?

A report sent by DataNode containing information about stored blocks.

Contains:

* Block IDs
* Replica Information
* Storage Details

---

## Purpose

Keeps NameNode metadata synchronized.

---

# Example Block Report

```text
DataNode 1

Block A

Block B

Block C
```

NameNode updates metadata accordingly.

---

# DataNode Startup Process

## Step 1

Read configuration files.

---

## Step 2

Load storage directories.

---

## Step 3

Register with NameNode.

---

## Step 4

Send initial block report.

---

## Step 5

Begin heartbeat communication.

---

# Data Replication

Default Replication Factor:

```text
3
```

Example:

```text
Block A

Copy 1 → DataNode 1

Copy 2 → DataNode 4

Copy 3 → DataNode 7
```

Purpose:

* Fault Tolerance
* High Availability

---

# DataNode Failure Scenario

## Scenario

DataNode 2 crashes.

---

## What Happens?

Heartbeat stops.

NameNode detects failure.

```text
DataNode 2 → DEAD
```

---

## Next Action

NameNode checks replication.

Example:

```text
Block A

Remaining Copies:

DataNode 1

DataNode 4
```

---

## Re-Replication

New replica created:

```text
DataNode 8
```

Replication restored automatically.

---

# DataNode Decommissioning

Sometimes nodes are removed intentionally.

Example:

* Hardware Upgrade
* Maintenance
* Cluster Resizing

---

## Safe Decommission Process

1. Mark node for decommission.
2. Replicate blocks elsewhere.
3. Remove node safely.

---

# DataNode Storage Management

## Check Disk Usage

```bash
df -h
```

---

## HDFS Usage

```bash
hdfs dfs -df -h
```

---

## DataNode Health

```bash
hdfs dfsadmin -report
```

---

# Common DataNode Issues

## Disk Full

Symptoms:

```text
Write failures
```

Solution:

* Free space
* Add disks
* Add nodes

---

## Network Failure

Symptoms:

```text
Heartbeat loss
```

Solution:

* Check connectivity
* Verify firewall rules

---

## Corrupted Blocks

Symptoms:

```text
FSCK errors
```

Check:

```bash
hdfs fsck /
```

---

# Real-World Example

A cluster contains:

```text
50 DataNodes
```

Each node:

```text
10 TB Storage
```

Total Cluster Capacity:

```text
500 TB
```

HDFS distributes blocks across all nodes.

Benefits:

* Scalability
* Parallel Processing
* Fault Tolerance

---

# Common Mistakes

## Mistake 1

Assuming DataNode manages metadata.

Reality:

Only NameNode manages metadata.

---

## Mistake 2

Ignoring dead DataNodes.

Check regularly:

```bash
hdfs dfsadmin -report
```

---

## Mistake 3

Not monitoring storage utilization.

Can lead to:

```text
Disk Full Errors
```

---

# Interview Questions

## Beginner

1. What is a DataNode?
2. What does a DataNode store?
3. What is the role of a DataNode?

---

## Intermediate

4. What is a heartbeat?
5. What is a block report?
6. How does a DataNode communicate with NameNode?

---

## Advanced

7. What happens when a DataNode fails?
8. Explain re-replication.
9. How does DataNode startup work?
10. How does HDFS maintain fault tolerance?

---

# Quick Revision

### DataNode

Worker Node

### Stores

Actual Data Blocks

### Sends

* Heartbeats
* Block Reports

### Heartbeat Frequency

~3 Seconds

### Purpose

Data Storage

### Failure Handling

Automatic Re-Replication

### Monitored By

NameNode

---

# Key Takeaways

* DataNodes store actual HDFS data blocks.
* They serve read and write requests.
* Heartbeats indicate node health.
* Block reports maintain metadata consistency.
* DataNode failures trigger automatic re-replication.
* Multiple DataNodes provide scalability and fault tolerance.
