# NameNode

## Introduction

The NameNode is the most critical component of HDFS.

It acts as the master server responsible for managing the entire HDFS namespace and metadata.

Without the NameNode, DataNodes would not know:

* Which blocks belong to which file
* Where files are located
* Replication information
* File permissions

For this reason, the NameNode is often called:

> **The Brain of HDFS**

---

# What is NameNode?

NameNode is the master daemon of HDFS responsible for managing filesystem metadata.

### Stores

* File Names
* Directory Structure
* Permissions
* Ownership
* Block Locations
* Replication Information

### Does NOT Store

❌ Actual Data

Actual data is stored in DataNodes.

---

# NameNode Architecture

```text
                    Client
                       |
                       v

                 +-----------+
                 | NameNode  |
                 +-----------+

                       |
        ----------------------------------

        |               |               |

        v               v               v

    DataNode1      DataNode2      DataNode3
```

The NameNode manages DataNodes and tracks all blocks stored in the cluster.

---

# Responsibilities of NameNode

## Metadata Management

Maintains complete filesystem metadata.

---

## Namespace Management

Manages:

* Files
* Directories
* Paths

Example:

```text
/user/onkar/project/data.csv
```

---

## Block Management

Tracks:

```text
data.csv

Block A → DataNode 1

Block B → DataNode 2

Block C → DataNode 3
```

---

## Replication Management

Ensures desired replication factor is maintained.

Example:

```text
Replication Factor = 3
```

If a DataNode fails:

* Detects missing replica
* Creates new replica

---

## Client Coordination

Clients contact NameNode for:

* File Read Requests
* File Write Requests
* Metadata Operations

---

# Metadata in NameNode

Example:

```text
File Name:
employee.csv

Owner:
onkar

Permission:
rw-r--r--

Replication:
3

Block Locations:
Block1 → DN1
Block2 → DN3
Block3 → DN5
```

This information is stored in NameNode memory.

---

# NameNode Memory Storage

One important interview question:

### Where does NameNode store metadata?

Answer:

Metadata is loaded into RAM.

Benefits:

* Fast access
* Quick lookup

Drawback:

* Memory limitation

---

# FsImage

## What is FsImage?

FsImage is a snapshot of the HDFS filesystem metadata.

Contains:

* Files
* Directories
* Permissions
* Ownership

Example:

```text
FsImage

/user
/project
/data
```

---

# Edit Logs

## What are Edit Logs?

Edit Logs record every change made to HDFS.

Examples:

```text
Create File

Delete File

Rename Directory

Change Permission
```

---

# Example

Suppose:

```bash
hdfs dfs -mkdir /project
```

This operation gets recorded in Edit Logs.

---

# Why Are Edit Logs Needed?

If NameNode crashes:

Changes can be recovered from Edit Logs.

---

# NameNode Startup Process

## Step 1

Load FsImage.

---

## Step 2

Load Edit Logs.

---

## Step 3

Apply Edit Logs to FsImage.

---

## Step 4

Create latest metadata state.

---

## Step 5

Start serving requests.

---

# Secondary NameNode

Purpose:

Merge:

```text
FsImage

+

Edit Logs
```

Creates a new checkpoint.

---

# Important Interview Point

### Is Secondary NameNode a Backup NameNode?

Answer:

❌ No

It only performs checkpointing.

---

# Heartbeat Monitoring

DataNodes send heartbeats to NameNode.

Default:

```text
~3 Seconds
```

Purpose:

Indicates DataNode is alive.

---

# What Happens If Heartbeat Stops?

NameNode assumes:

```text
DataNode Failed
```

Actions:

1. Mark node as dead
2. Detect under-replicated blocks
3. Start re-replication

---

# Block Reports

DataNodes periodically send block reports.

Contains:

* Block IDs
* Replica Information
* Storage Information

Purpose:

Keeps NameNode metadata accurate.

---

# NameNode Failure Scenario

## Scenario

NameNode crashes.

---

## Impact

Clients cannot:

* Read Files
* Write Files
* Access Metadata

Even though DataNodes still have data.

---

## Why?

Because block locations are managed by NameNode.

---

# High Availability Solution

Modern Hadoop clusters use:

```text
Active NameNode

Standby NameNode
```

Benefits:

* No single point of failure
* Faster recovery

---

# NameNode Best Practices

## Monitor Memory Usage

Metadata resides in RAM.

---

## Backup Metadata

Protect:

* FsImage
* Edit Logs

---

## Configure High Availability

Avoid single point of failure.

---

## Monitor Logs

Check:

```bash
$HADOOP_HOME/logs
```

---

# Real-World Example

Suppose a company stores:

```text
500 Million Files
```

Each file requires metadata.

NameNode RAM usage becomes very high.

This is one reason why:

```text
Small Files Problem
```

is dangerous in HDFS.

---

# Common Mistakes

## Mistake 1

Assuming NameNode stores data.

Reality:

Only metadata is stored.

---

## Mistake 2

Thinking Secondary NameNode is backup.

Reality:

Checkpointing service only.

---

## Mistake 3

Ignoring NameNode memory usage.

Can lead to:

* Performance degradation
* Cluster failure

---

# Interview Questions

## Beginner

1. What is NameNode?
2. What does NameNode store?
3. Does NameNode store actual data?

---

## Intermediate

4. What is metadata?
5. What is FsImage?
6. What are Edit Logs?
7. What is checkpointing?

---

## Advanced

8. Explain NameNode startup process.
9. What happens if NameNode crashes?
10. How does High Availability solve NameNode failure?
11. Why is RAM important for NameNode?

---

# Quick Revision

### NameNode

Master of HDFS

### Stores

Metadata

### Does NOT Store

Actual Data

### Metadata Files

* FsImage
* Edit Logs

### Heartbeat

~3 Seconds

### Role of Secondary NameNode

Checkpointing

### Biggest Challenge

Metadata stored in RAM

### Solution for NameNode Failure

High Availability (HA)

---

# Key Takeaways

* NameNode is the brain of HDFS.
* It manages metadata and namespace.
* Metadata is stored in memory for fast access.
* FsImage stores filesystem snapshots.
* Edit Logs store filesystem changes.
* Secondary NameNode performs checkpointing.
* NameNode failure can make HDFS inaccessible.
* High Availability removes the single point of failure.
