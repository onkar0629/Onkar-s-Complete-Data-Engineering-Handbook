# HDFS Architecture

## Introduction

HDFS (Hadoop Distributed File System) follows a Master-Slave architecture designed to store massive datasets across multiple machines while ensuring fault tolerance, scalability, and high availability.

The architecture separates metadata management from data storage, allowing HDFS to efficiently manage petabytes of data.

---

# HDFS Architecture Overview

```text
                    Client
                       |
                       |
                       v
                +-------------+
                |  NameNode   |
                |  (Master)   |
                +-------------+
                       |
        ---------------------------------
        |               |               |
        v               v               v
   +---------+     +---------+     +---------+
   |DataNode1|     |DataNode2|     |DataNode3|
   +---------+     +---------+     +---------+
```

---

# Components of HDFS

HDFS consists of two primary components:

## 1. NameNode (Master)

The NameNode is the brain of HDFS.

It manages:

* Metadata
* Namespace
* File System Tree
* Block Locations
* Permissions

### Responsibilities

* Tracks files and directories
* Maintains block mapping
* Handles client requests
* Manages replication

### Important Point

The NameNode does NOT store actual data.

It only stores metadata.

---

# What Metadata Contains

Example:

```text
File Name: employee.csv

Owner: hdfs

Permission: rw-r--r--

Blocks:
Block A -> DataNode1
Block B -> DataNode2
Block C -> DataNode3
```

This information is stored by the NameNode.

---

## 2. DataNode (Worker)

DataNodes store actual data blocks.

### Responsibilities

* Store blocks
* Serve read requests
* Serve write requests
* Send Heartbeats
* Send Block Reports

---

# Master-Slave Architecture

```text
Master
  |
  |---- NameNode
  |
Workers
  |
  |---- DataNode 1
  |---- DataNode 2
  |---- DataNode 3
```

### Benefits

* Centralized metadata management
* Distributed data storage
* Easy scalability

---

# File Storage in HDFS

Suppose a file size is:

```text
300 MB
```

Default block size:

```text
128 MB
```

HDFS divides file into:

```text
Block 1 = 128 MB

Block 2 = 128 MB

Block 3 = 44 MB
```

Blocks are distributed across DataNodes.

---

# Block Distribution Example

```text
Block 1 → DataNode 1

Block 2 → DataNode 2

Block 3 → DataNode 3
```

This enables parallel processing.

---

# Replication Architecture

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

# Heartbeat Mechanism

Every DataNode periodically sends a heartbeat to the NameNode.

### Purpose

Indicates that DataNode is alive.

### Default Interval

Approximately:

```text
3 Seconds
```

---

# What Happens If Heartbeat Stops?

NameNode assumes:

```text
DataNode Failed
```

Actions:

1. Marks node as dead
2. Identifies under-replicated blocks
3. Starts re-replication

---

# Block Report

DataNodes periodically send block reports.

### Contains

* Stored block list
* Block status
* Replica information

### Purpose

Allows NameNode to maintain accurate metadata.

---

# NameNode Metadata Files

NameNode stores metadata in:

## FsImage

Snapshot of filesystem metadata.

---

## Edit Logs

Records all filesystem changes.

Examples:

* File creation
* File deletion
* Directory creation

---

# Secondary NameNode

Purpose:

Merge:

```text
FsImage + Edit Logs
```

Creates a new checkpoint.

### Important Interview Point

Secondary NameNode is NOT a backup NameNode.

---

# Client Interaction

Clients never communicate directly with DataNodes initially.

Process:

```text
Client
   |
   v
NameNode
   |
   v
DataNodes
```

NameNode provides block locations.

Client then accesses DataNodes directly.

---

# HDFS Read Workflow

## Step 1

Client requests file.

---

## Step 2

NameNode returns block locations.

---

## Step 3

Client contacts nearest DataNode.

---

## Step 4

Data blocks are retrieved.

---

# HDFS Write Workflow

## Step 1

Client requests file creation.

---

## Step 2

NameNode allocates blocks.

---

## Step 3

Client writes data to DataNodes.

---

## Step 4

Replication occurs.

---

## Step 5

Metadata updated.

---

# Fault Tolerance

HDFS survives failures using:

* Replication
* Heartbeats
* Block Reports

Example:

```text
DataNode 2 Fails
```

Result:

```text
Replica on DataNode 4 and 7 remain available
```

No data loss.

---

# Rack Awareness

Hadoop stores replicas across multiple racks.

Example:

```text
Rack 1
  DataNode 1
  DataNode 2

Rack 2
  DataNode 3
  DataNode 4
```

Benefits:

* Better Fault Tolerance
* Protection Against Rack Failure

---

# Data Locality

HDFS moves computation closer to data.

Instead of:

```text
Move Data → Processing
```

HDFS uses:

```text
Move Processing → Data
```

Benefits:

* Faster Execution
* Reduced Network Traffic

---

# Real-World Example

Netflix stores petabytes of viewing history.

Data is:

* Split into blocks
* Distributed across DataNodes
* Replicated for safety
* Processed in parallel

This allows efficient large-scale analytics.

---

# Advantages of HDFS Architecture

* Fault Tolerant
* Highly Scalable
* Cost Effective
* Distributed Storage
* High Throughput
* Data Locality

---

# Limitations of HDFS Architecture

* NameNode dependency
* Not suitable for small files
* Not designed for low-latency access
* Write-once-read-many model

---

# Interview Questions

## Beginner

1. Explain HDFS Architecture.
2. What is NameNode?
3. What is DataNode?

## Intermediate

4. What is a heartbeat?
5. What is a block report?
6. What is metadata?

## Advanced

7. What happens when a DataNode fails?
8. Why is replication important?
9. Explain rack awareness.
10. Explain data locality.

---

# Key Takeaways

* HDFS follows a Master-Slave architecture.
* NameNode manages metadata.
* DataNodes store actual data blocks.
* Files are split into blocks.
* Replication provides fault tolerance.
* Heartbeats monitor DataNode health.
* Block reports help maintain metadata consistency.
* Data locality improves performance.
* Rack awareness improves reliability.
