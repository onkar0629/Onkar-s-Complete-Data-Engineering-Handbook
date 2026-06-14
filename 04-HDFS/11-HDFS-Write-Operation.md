# HDFS Write Operation

## Introduction

The HDFS Write Operation describes how data is stored in HDFS.

Unlike traditional file systems, HDFS does not store a file on a single machine. Instead:

* File is split into blocks
* Blocks are distributed across DataNodes
* Replicas are created automatically

The write operation is designed to provide:

* Fault Tolerance
* Reliability
* Scalability
* High Throughput

This is one of the most important HDFS interview topics.

---

# Components Involved

## Client

The user or application writing data.

---

## NameNode

Responsible for:

* Metadata management
* Block allocation
* Replica placement

---

## DataNodes

Responsible for:

* Storing actual blocks
* Creating replicas
* Sending acknowledgements

---

# High-Level Workflow

```text
          Client
             |
             v
         NameNode
             |
             v
     Block Locations
             |
             v
       DataNodes
```

---

# Example

Suppose we upload:

```text
sales.csv
```

File Size:

```text
300 MB
```

Block Size:

```text
128 MB
```

Blocks created:

```text
Block A = 128 MB

Block B = 128 MB

Block C = 44 MB
```

---

# Step-by-Step HDFS Write Operation

## Step 1: Client Requests File Creation

Client sends request:

```text
Create sales.csv
```

to NameNode.

---

## Step 2: NameNode Checks

NameNode verifies:

* File already exists?
* User permissions?
* Namespace validity?

If successful:

```text
File Creation Approved
```

---

## Step 3: NameNode Selects DataNodes

Assume Replication Factor:

```text
3
```

NameNode selects:

```text
DN1

DN4

DN7
```

for Block A.

---

## Step 4: NameNode Returns Pipeline

NameNode returns:

```text
Block A

DN1 → DN4 → DN7
```

This is called the:

### Replication Pipeline

---

# Pipeline Architecture

```text
Client
   |
   v
 DN1
   |
   v
 DN4
   |
   v
 DN7
```

---

## Step 5: Client Sends Data

Client starts sending:

```text
Block A
```

to:

```text
DN1
```

---

## Step 6: Data Forwarding

DN1 stores block locally.

Then forwards block to:

```text
DN4
```

---

DN4 stores block.

Then forwards to:

```text
DN7
```

---

DN7 stores block.

Replication complete.

---

# Acknowledgement Flow

After successful storage:

```text
DN7 → DN4 → DN1 → Client
```

Acknowledgements travel in reverse direction.

---

# ACK Flow Diagram

```text
Write Flow

Client

↓

DN1

↓

DN4

↓

DN7
```

```text
ACK Flow

Client

↑

DN1

↑

DN4

↑

DN7
```

---

# Block Completion

After Block A completes:

NameNode allocates DataNodes for:

```text
Block B
```

and later:

```text
Block C
```

Same process repeats.

---

# Final Metadata Update

After all blocks are written:

NameNode updates metadata.

Example:

```text
sales.csv

Block A → DN1 DN4 DN7

Block B → DN2 DN5 DN8

Block C → DN3 DN6 DN9
```

---

# Write Operation Architecture

```text
                  Client
                     |
                     |
                     v

                 NameNode

                     |

     ---------------------------------

     |               |              |

     v               v              v

    DN1             DN4            DN7

   Replica 1      Replica 2      Replica 3
```

---

# Why Use a Pipeline?

Benefits:

* Efficient replication
* Reduced network traffic
* Better scalability

Without a pipeline:

```text
Client

↓

DN1

Client

↓

DN4

Client

↓

DN7
```

Client would send data three times.

Very inefficient.

---

# Write Operation and Replication

Replication Factor:

```text
3
```

Means:

```text
1 Original

2 Replicas
```

All replicas created during write operation.

---

# Failure Scenario 1

## DataNode Failure During Write

Pipeline:

```text
DN1

↓

DN4

↓

DN7
```

DN4 crashes.

---

# What Happens?

NameNode detects failure.

Pipeline reconfigured.

Example:

```text
DN1

↓

DN8
```

New replica created.

Write continues.

---

# Failure Scenario 2

## Network Failure

If network breaks:

```text
Client ↔ DN1
```

Write operation pauses.

Retry mechanisms begin.

---

# Failure Scenario 3

## Disk Full

DataNode storage exhausted.

NameNode selects another DataNode.

Write operation continues.

---

# Temporary Data Buffering

Client does not send entire block at once.

Data is divided into:

### Packets

Example:

```text
Block A

Packet 1

Packet 2

Packet 3
```

This improves efficiency.

---

# Data Integrity

HDFS uses:

### Checksums

Purpose:

Detect corruption.

If corruption occurs:

Replica from another DataNode is used.

---

# Real-World Example

Suppose Netflix uploads:

```text
10 TB
```

of viewing logs daily.

Process:

```text
Logs

↓

Blocks

↓

Distributed Across Hundreds of DataNodes

↓

Replicated
```

Result:

* Reliable Storage
* Fault Tolerance
* High Availability

---

# Write Operation vs Traditional File System

| Traditional FS      | HDFS                  |
| ------------------- | --------------------- |
| Single Server Write | Distributed Write     |
| No Replication      | Automatic Replication |
| Limited Scalability | Highly Scalable       |
| Single Failure Risk | Fault Tolerant        |

---

# Common Mistakes

## Mistake 1

Thinking NameNode stores data.

Reality:

Only metadata.

---

## Mistake 2

Assuming client writes directly to all replicas.

Reality:

Pipeline replication is used.

---

## Mistake 3

Ignoring acknowledgement flow.

ACKs are critical for write confirmation.

---

# Interview Scenario

## Scenario

Replication Factor:

```text
3
```

Pipeline:

```text
DN1

↓

DN4

↓

DN7
```

DN4 fails.

Question:

What happens?

Answer:

NameNode reconfigures pipeline and creates a new replica on another DataNode.

---

# Interview Questions

## Beginner

1. Explain HDFS Write Operation.
2. What is the role of NameNode during write?

---

## Intermediate

3. What is a replication pipeline?
4. Why does HDFS use acknowledgements?

---

## Advanced

5. Explain complete write workflow.
6. What happens if a DataNode fails during write?
7. How does HDFS maintain consistency?
8. Why is a pipeline better than direct replication?
9. How are checksums used?
10. How does NameNode choose DataNodes?

---

# Quick Revision

### Step 1

Client contacts NameNode

### Step 2

NameNode selects DataNodes

### Step 3

Pipeline created

### Step 4

Data written

### Step 5

Replication completed

### Step 6

ACK received

### Step 7

Metadata updated

### NameNode Stores

Metadata

### DataNodes Store

Actual Blocks

### Replication Method

Pipeline

### Integrity Check

Checksum

---

# Key Takeaways

* HDFS Write Operation begins with NameNode metadata allocation.
* Data is written to DataNodes through a replication pipeline.
* Acknowledgements confirm successful storage.
* Replication provides fault tolerance.
* Checksums ensure data integrity.
* Pipeline architecture reduces network overhead.
* HDFS can continue writing even if some DataNodes fail.
