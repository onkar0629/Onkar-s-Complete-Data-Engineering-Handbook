# HDFS Cheat Sheet (5-Minute Revision)

> Use this file before interviews, exams, certifications, or practicals.

---

# 1. HDFS Architecture

```text
            NameNode
               |
      -------------------
      |        |        |
     DN1      DN2      DN3
```

### NameNode

* Master Node
* Stores Metadata
* Maintains Block Locations
* Manages Replication

### DataNode

* Worker Node
* Stores Actual Data Blocks
* Sends Heartbeats
* Sends Block Reports

---

# 2. Metadata Files

### FsImage

```text
Snapshot of Metadata
```

Contains:

* Files
* Directories
* Permissions

---

### Edit Logs

```text
Changes to Metadata
```

Examples:

* Create File
* Delete File
* Rename File

---

# 3. Blocks

### Definition

Smallest Storage Unit in HDFS

### Default Block Size

```text
128 MB
```

### Example

```text
300 MB File

↓

128 MB

↓

128 MB

↓

44 MB
```

---

# 4. Replication

### Definition

Multiple copies of blocks.

### Default Replication Factor

```text
3
```

### Benefits

* Fault Tolerance
* High Availability

---

# 5. Heartbeat

### Purpose

DataNode tells NameNode:

```text
I Am Alive
```

### Frequency

```text
~3 Seconds
```

### Missing Heartbeat

```text
DataNode Marked Dead
```

---

# 6. Block Report

Contains:

* Block IDs
* Replica Information

Purpose:

```text
Metadata Synchronization
```

---

# 7. Secondary NameNode

### Purpose

Checkpointing

```text
FsImage

+

Edit Logs

↓

New FsImage
```

### Interview Trap

❌ Backup NameNode

✅ Checkpointing Service

---

# 8. HDFS Read Operation

```text
Client

↓

NameNode

↓

Block Locations

↓

DataNodes

↓

Client
```

### Remember

NameNode sends:

```text
Metadata
```

DataNodes send:

```text
Actual Data
```

---

# 9. HDFS Write Operation

```text
Client

↓

NameNode

↓

DN1

↓

DN2

↓

DN3
```

### ACK Flow

```text
DN3

↑

DN2

↑

DN1

↑

Client
```

---

# 10. Rack Awareness

### Purpose

Store replicas across racks.

Example:

```text
Replica 1 → Rack 1

Replica 2 → Rack 2

Replica 3 → Rack 2
```

### Benefit

Protection against rack failure.

---

# 11. Data Locality

### Concept

```text
Move Computation To Data
```

NOT

```text
Move Data To Computation
```

### Types

1. Node Local
2. Rack Local
3. Off Rack

---

# 12. High Availability (HA)

### Components

```text
Active NameNode

Standby NameNode

ZooKeeper

JournalNodes
```

### Purpose

Remove:

```text
Single Point Of Failure
```

---

# 13. Federation

### Purpose

Scalability

### Components

```text
Multiple NameNodes

Multiple Namespaces

Multiple Block Pools
```

### Interview Trap

HA ≠ Federation

---

# 14. Important Commands

## List Files

```bash
hdfs dfs -ls /
```

## Create Directory

```bash
hdfs dfs -mkdir /data
```

## Upload File

```bash
hdfs dfs -put file.csv /data
```

## Download File

```bash
hdfs dfs -get /data/file.csv .
```

## View File

```bash
hdfs dfs -cat /data/file.csv
```

## Delete File

```bash
hdfs dfs -rm file.csv
```

## Delete Directory

```bash
hdfs dfs -rm -r /data
```

## Change Replication

```bash
hdfs dfs -setrep 3 file.csv
```

---

# 15. Admin Commands

## Cluster Report

```bash
hdfs dfsadmin -report
```

## Safe Mode Status

```bash
hdfs dfsadmin -safemode get
```

## Leave Safe Mode

```bash
hdfs dfsadmin -safemode leave
```

## Health Check

```bash
hdfs fsck /
```

## Balance Cluster

```bash
hdfs balancer
```

---

# 16. Troubleshooting Commands

## Running Services

```bash
jps
```

## Disk Usage

```bash
df -h
```

## Memory Usage

```bash
free -m
```

## Cluster Health

```bash
hdfs dfsadmin -report
```

## File System Health

```bash
hdfs fsck /
```

---

# 17. Common Production Problems

| Problem             | Solution         |
| ------------------- | ---------------- |
| Small Files         | Parquet / ORC    |
| DataNode Failure    | Re-Replication   |
| NameNode Failure    | HA               |
| Rack Failure        | Rack Awareness   |
| Disk Full           | Add Nodes        |
| Metadata Bottleneck | Federation       |
| Cluster Imbalance   | Balancer         |
| Corrupt Block       | Replica Recovery |

---

# 18. Interview Traps

### Does NameNode store data?

❌ No

✅ Stores Metadata

---

### Is Secondary NameNode a Backup?

❌ No

✅ Checkpointing Service

---

### Does HA provide Scalability?

❌ No

✅ Fault Tolerance

---

### Does Federation provide Fault Tolerance?

❌ No

✅ Scalability

---

### Can HDFS survive DataNode failure?

✅ Yes

Replication

---

### Can HDFS survive NameNode failure without HA?

❌ No

---

# 19. Most Important Numbers

| Item                       | Value  |
| -------------------------- | ------ |
| Default Block Size         | 128 MB |
| Default Replication Factor | 3      |
| Heartbeat Interval         | ~3 sec |
| NameNode UI Port           | 9870   |
| DataNode UI Port           | 9864   |
| ResourceManager UI         | 8088   |

---

# 20. 30-Second Interview Revision

```text
NameNode → Metadata

DataNode → Actual Data

Block Size → 128 MB

Replication → 3

Heartbeat → Alive Signal

Block Report → Block Info

Secondary NN → Checkpointing

Rack Awareness → Cross Rack Replicas

Data Locality → Move Compute To Data

HA → Fault Tolerance

Federation → Scalability

FSCK → Health Check

Balancer → Cluster Balancing

Small Files → Metadata Problem
```

---

# HDFS Master Formula

```text
Files
 ↓
Blocks
 ↓
DataNodes
 ↓
Replication
 ↓
Rack Awareness
 ↓
Data Locality
 ↓
High Availability
 ↓
Federation
```

