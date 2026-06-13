# Introduction to HDFS

## What is HDFS?

HDFS (Hadoop Distributed File System) is the distributed storage layer of Hadoop designed to store and manage very large datasets across multiple machines.

It allows data to be stored reliably on clusters of commodity hardware while providing:

* Fault Tolerance
* Scalability
* High Throughput
* Distributed Storage

HDFS is optimized for Big Data workloads where files are typically very large and accessed sequentially.

---

# Full Form

**HDFS = Hadoop Distributed File System**

---

# Why Was HDFS Created?

Traditional file systems were designed for storing and processing data on a single machine.

As organizations started generating terabytes and petabytes of data, traditional systems faced several challenges:

## Storage Limitation

A single machine has limited storage capacity.

Example:

```text
1 Server = 20 TB Storage
```

What if a company needs:

```text
500 TB Storage?
```

A single machine is not enough.

---

## Performance Limitation

Processing huge datasets on a single machine becomes slow.

---

## High Cost

Increasing storage requires purchasing expensive hardware.

---

## Single Point of Failure

If the server crashes:

* Data becomes unavailable
* Business operations may stop

---

# Inspiration Behind HDFS

HDFS was inspired by:

## Google File System (GFS)

Google published a research paper explaining how it stored massive amounts of data across clusters.

The Hadoop community used these ideas to build HDFS.

---

# Design Goals of HDFS

HDFS was designed to provide:

## 1. Reliability

Data should remain available even if machines fail.

---

## 2. Scalability

Storage should grow easily by adding more machines.

---

## 3. Fault Tolerance

System should continue operating despite failures.

---

## 4. High Throughput

Optimized for processing large files efficiently.

---

## 5. Cost Effectiveness

Runs on commodity hardware.

---

# How HDFS Solves These Problems

Instead of storing files on one machine:

```text
Traditional System

Server
   |
   └── Entire File
```

HDFS stores files across multiple machines.

```text
HDFS

Block 1 -> DataNode 1

Block 2 -> DataNode 2

Block 3 -> DataNode 3
```

This enables distributed storage and processing.

---

# Key Characteristics of HDFS

## Distributed Storage

Files are divided into blocks and distributed across DataNodes.

---

## Large Block Size

Default Block Size:

```text
128 MB
```

Large blocks reduce metadata overhead and improve performance.

---

## Replication

Multiple copies of blocks are stored.

Default Replication Factor:

```text
3
```

---

## Fault Tolerance

If one DataNode fails:

* Other replicas remain available.
* Data is not lost.

---

## Streaming Data Access

HDFS is optimized for:

```text
Write Once
Read Many
```

workloads.

---

# HDFS Architecture Overview

HDFS follows a Master-Slave architecture.

## Master

### NameNode

Responsible for:

* Metadata Management
* Block Tracking
* Namespace Management

---

## Workers

### DataNodes

Responsible for:

* Storing Blocks
* Serving Read Requests
* Serving Write Requests

---

# Example

Suppose we upload a file:

```text
sales.csv
Size = 300 MB
```

With a block size of:

```text
128 MB
```

HDFS creates:

```text
Block 1 = 128 MB

Block 2 = 128 MB

Block 3 = 44 MB
```

These blocks are distributed across DataNodes.

---

# Advantages of HDFS

## Fault Tolerance

Data remains available during node failures.

---

## Scalability

Storage grows by adding nodes.

---

## Cost Effective

Uses commodity hardware.

---

## High Throughput

Efficient for large-scale analytics.

---

## Data Locality

Processing occurs close to data.

---

# Limitations of HDFS

## Not Suitable for Small Files

Millions of small files create metadata overhead.

---

## High Latency

Not designed for real-time systems.

---

## Write Once Read Many

Does not support frequent random updates efficiently.

---

## Metadata Dependency

NameNode manages metadata and is critical to the cluster.

---

# Real-World Applications

## Netflix

Stores viewing history and recommendation data.

---

## Amazon

Stores customer activity and transaction logs.

---

## Facebook

Stores user interaction data.

---

## Banking

Stores transaction and audit records.

---

## Healthcare

Stores medical records and research datasets.

---

# Interview Questions

## Beginner

1. What is HDFS?
2. What is the full form of HDFS?
3. Why was HDFS created?

---

## Intermediate

4. What are the design goals of HDFS?
5. Why does HDFS use large block sizes?
6. What is the default replication factor?

---

## Advanced

7. How does HDFS achieve fault tolerance?
8. Why is HDFS optimized for large files?
9. What are the limitations of HDFS?

---

# Key Takeaways

* HDFS is Hadoop's distributed storage layer.
* It was inspired by Google File System (GFS).
* HDFS solves storage, scalability, and fault-tolerance challenges.
* Files are divided into blocks and distributed across DataNodes.
* Replication provides fault tolerance.
* HDFS is designed for large-scale Big Data workloads.
