# Hadoop Cheat Sheet

## Hadoop in One Line

Apache Hadoop is an open-source framework used for distributed storage and distributed processing of Big Data across clusters of computers.

---

# Hadoop Core Components

| Component | Purpose             |
| --------- | ------------------- |
| HDFS      | Storage             |
| YARN      | Resource Management |
| MapReduce | Processing          |

---

# Hadoop Architecture

```text
                Client
                   |
                   v

             NameNode
        (Metadata Manager)

                   |

     -------------------------
     |           |           |

  DataNode    DataNode    DataNode

  Stores      Stores      Stores
  Blocks      Blocks      Blocks
```

---

# HDFS Quick Revision

### HDFS

Hadoop Distributed File System

### Purpose

Distributed storage.

### Features

* Fault Tolerance
* Replication
* Scalability
* Data Locality

---

# Important Numbers

| Parameter          | Value      |
| ------------------ | ---------- |
| Block Size         | 128 MB     |
| Replication Factor | 3          |
| Heartbeat          | ~3 Seconds |
| Block Report       | ~1 Hour    |

---

# NameNode

### Stores

Metadata

### Contains

* File Names
* Permissions
* Block Locations
* Replication Information

### Does NOT Store

Actual Data

---

# DataNode

### Stores

Actual Data Blocks

### Responsibilities

* Block Storage
* Heartbeats
* Block Reports

---

# Secondary NameNode

### Purpose

Merges:

* FsImage
* Edit Logs

### Important

❌ Not Backup NameNode

---

# YARN

### Full Form

Yet Another Resource Negotiator

### Purpose

Resource Management

### Components

* ResourceManager
* NodeManager
* ApplicationMaster

---

# MapReduce

### Phases

1. Map
2. Shuffle & Sort
3. Reduce

---

# Data Locality

### Definition

Move computation to data rather than moving data to computation.

### Benefit

Reduced network traffic.

---

# Rack Awareness

### Definition

Stores replicas across different racks.

### Benefit

Improved fault tolerance.

---

# Fault Tolerance

### Achieved By

Replication

Example:

```text
Block A

Copy 1 → DataNode 1
Copy 2 → DataNode 5
Copy 3 → DataNode 8
```

---

# HDFS Read Operation

1. Client requests file.
2. NameNode returns block locations.
3. Client reads blocks from DataNodes.

---

# HDFS Write Operation

1. Client contacts NameNode.
2. NameNode returns DataNodes.
3. Client writes blocks.
4. Replication occurs.
5. Metadata updated.

---

# Common HDFS Commands

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
hdfs dfs -rm /data/file.csv
```

---

# Cluster Commands

## Cluster Health

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

## Check Blocks

```bash
hdfs fsck /
```

## Running Services

```bash
jps
```

---

# Most Asked Interview Questions

### What is Hadoop?

Distributed storage and processing framework.

---

### What is HDFS?

Distributed storage layer.

---

### What is YARN?

Resource management layer.

---

### What is MapReduce?

Distributed processing framework.

---

### What does NameNode store?

Metadata.

---

### What does DataNode store?

Actual Data Blocks.

---

### What is Data Locality?

Processing data where it resides.

---

### What is Rack Awareness?

Distributing replicas across racks.

---

### What is Safe Mode?

Read-only state of NameNode.

---

### What is Fault Tolerance?

Ability to continue operating despite failures.

---

# Top Interview One-Liners

### Default Block Size

128 MB

### Default Replication Factor

3

### NameNode Stores

Metadata

### DataNode Stores

Actual Data

### Hadoop Scaling

Horizontal Scaling

### Processing Framework

MapReduce

### Resource Manager

YARN

### Storage Layer

HDFS

### Fault Tolerance

Replication

### Biggest HDFS Problem

Small Files Problem

---

# 30-Second Revision

✅ Hadoop = Distributed Storage + Distributed Processing

✅ HDFS = Storage

✅ YARN = Resource Management

✅ MapReduce = Processing

✅ NameNode = Metadata

✅ DataNode = Actual Data

✅ Block Size = 128 MB

✅ Replication Factor = 3

✅ Heartbeat = 3 sec

✅ Horizontal Scaling

✅ Fault Tolerance via Replication

✅ Data Locality improves performance

✅ Rack Awareness improves reliability
