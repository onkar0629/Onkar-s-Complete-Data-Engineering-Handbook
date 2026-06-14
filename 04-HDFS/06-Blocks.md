# HDFS Blocks

## Introduction

Blocks are the fundamental storage units of HDFS.

Unlike traditional file systems that store a file as a single unit, HDFS divides files into smaller pieces called **blocks** and distributes them across multiple DataNodes.

This design enables:

* Distributed Storage
* Parallel Processing
* Fault Tolerance
* Scalability

Understanding blocks is essential because almost every HDFS feature depends on them.

---

# What is a Block?

A block is the minimum unit of storage in HDFS.

When a file is stored in HDFS, it is split into one or more blocks.

Example:

```text
File Size = 300 MB

Block Size = 128 MB
```

Result:

```text
Block 1 = 128 MB

Block 2 = 128 MB

Block 3 = 44 MB
```

---

# Why Does HDFS Use Blocks?

Traditional file systems store files on a single machine.

Problems:

* Limited storage
* Poor scalability
* No distributed processing

HDFS uses blocks to:

* Store files across multiple machines
* Enable parallel processing
* Improve fault tolerance

---

# Default Block Size

The default block size in modern Hadoop versions is:

```text
128 MB
```

Older Hadoop versions commonly used:

```text
64 MB
```

---

# Why Large Block Sizes?

Large blocks reduce:

* Metadata overhead
* Disk seek operations

Benefits:

* Better throughput
* Faster processing
* Lower NameNode memory usage

---

# Block Creation Example

Suppose we upload:

```text
sales_data.csv

Size = 500 MB
```

Block Size:

```text
128 MB
```

Blocks created:

```text
Block 1 = 128 MB

Block 2 = 128 MB

Block 3 = 128 MB

Block 4 = 116 MB
```

---

# Block Distribution

HDFS distributes blocks across DataNodes.

Example:

```text
Block 1 → DataNode 1

Block 2 → DataNode 2

Block 3 → DataNode 3

Block 4 → DataNode 4
```

This enables parallel processing.

---

# Block Metadata

The NameNode stores metadata about every block.

Example:

```text
sales_data.csv

Block 1 → DataNode 1

Block 2 → DataNode 2

Block 3 → DataNode 3
```

Only metadata is stored in NameNode.

Actual blocks remain in DataNodes.

---

# Block Replication

Each block is replicated.

Default Replication Factor:

```text
3
```

Example:

```text
Block A

Copy 1 → DataNode 1

Copy 2 → DataNode 5

Copy 3 → DataNode 8
```

Purpose:

* Fault Tolerance
* High Availability

---

# What Happens If a DataNode Fails?

Suppose:

```text
Block A

Stored On:

DataNode 1
DataNode 5
DataNode 8
```

DataNode 5 crashes.

Remaining copies:

```text
DataNode 1
DataNode 8
```

HDFS automatically creates a new replica.

No data loss occurs.

---

# Large Files in HDFS

Example:

```text
File Size = 1 GB
```

Block Size:

```text
128 MB
```

Blocks:

```text
1024 / 128 = 8 Blocks
```

HDFS distributes these blocks across the cluster.

Benefits:

* Faster processing
* Better resource utilization

---

# Small Files Problem

One of the biggest HDFS challenges.

Example:

```text
1,000,000 Files

Each = 10 KB
```

---

# Why Is This a Problem?

Every file requires metadata.

NameNode stores:

* File Name
* Permissions
* Block Information

Millions of files create massive metadata overhead.

---

# Impact

* Increased RAM usage
* NameNode memory pressure
* Slower performance

---

# Solution

Use:

* Parquet
* ORC
* Avro
* Sequence Files

Merge small files whenever possible.

---

# Block Placement Strategy

When writing data:

HDFS decides:

```text
Which DataNode?

Which Rack?

How Many Replicas?
```

Goals:

* Fault Tolerance
* Load Balancing
* Data Locality

---

# Block Reports

DataNodes periodically send block reports.

Contains:

```text
Block IDs

Replica Information

Storage Status
```

Purpose:

Keep NameNode metadata synchronized.

---

# Checking Block Information

Command:

```bash
hdfs fsck /data/file.csv -files -blocks
```

Output shows:

* Blocks
* Locations
* Replicas

---

# Real-World Example

Suppose Netflix uploads:

```text
Movie Analytics Data

Size = 10 TB
```

Block Size:

```text
128 MB
```

Number of Blocks:

```text
10 TB ÷ 128 MB

≈ 81,920 Blocks
```

These blocks are distributed across hundreds of DataNodes.

Benefits:

* Parallel Processing
* Fault Tolerance
* Scalability

---

# Common Mistakes

## Mistake 1

Assuming blocks are files.

Reality:

Files are divided into blocks.

---

## Mistake 2

Using tiny files.

Leads to:

```text
Small Files Problem
```

---

## Mistake 3

Changing block size without understanding impact.

Can reduce performance.

---

# Interview Questions

## Beginner

1. What is a block in HDFS?
2. What is the default block size?
3. Why does HDFS use blocks?

---

## Intermediate

4. How are files stored in HDFS?
5. Why are large block sizes used?
6. What metadata is stored for blocks?

---

## Advanced

7. Explain the Small Files Problem.
8. How does HDFS distribute blocks?
9. What happens if a block replica is lost?
10. How do block reports help NameNode?

---

# Quick Revision

### Block

Smallest storage unit in HDFS

### Default Size

128 MB

### Stored In

DataNodes

### Managed By

NameNode

### Replication Factor

3

### Main Benefit

Distributed Storage

### Biggest Block Issue

Small Files Problem

### Check Block Information

```bash
hdfs fsck <file> -files -blocks
```

---

# Key Takeaways

* Blocks are the fundamental storage units of HDFS.
* Files are divided into blocks before storage.
* Default block size is 128 MB.
* Blocks are distributed across DataNodes.
* Replication provides fault tolerance.
* Large blocks improve throughput.
* Small files create metadata challenges.
* Understanding blocks is essential for understanding HDFS.
