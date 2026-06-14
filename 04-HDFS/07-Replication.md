# HDFS Replication

## Introduction

Replication is one of the most important features of HDFS.

It ensures that data remains available even when hardware failures occur.

Since Hadoop clusters are built using commodity hardware, failures are expected rather than exceptional.

HDFS solves this problem by storing multiple copies of each block across different DataNodes.

This mechanism is known as **Replication**.

---

# What is Replication?

Replication is the process of storing multiple copies of the same data block on different DataNodes.

Example:

```text
Block A

Copy 1 → DataNode 1

Copy 2 → DataNode 5

Copy 3 → DataNode 8
```

If one DataNode fails, other copies remain available.

---

# Why is Replication Needed?

Without replication:

```text
Block A

Stored Only On:

DataNode 1
```

If DataNode 1 crashes:

```text
Data Lost
```

With replication:

```text
Block A

DataNode 1

DataNode 5

DataNode 8
```

Even if one node fails, data remains accessible.

---

# Replication Factor

The number of copies maintained for a block.

Formula:

```text
Replication Factor = Number of Block Copies
```

---

# Default Replication Factor

```text
3
```

This means every block has:

```text
1 Original Copy

2 Additional Copies
```

Total:

```text
3 Copies
```

---

# Example

File:

```text
employee.csv

Size = 300 MB
```

Block Size:

```text
128 MB
```

Blocks:

```text
Block 1

Block 2

Block 3
```

Each block is replicated 3 times.

---

# Storage Example

```text
Block 1

Replica 1 → DN1

Replica 2 → DN4

Replica 3 → DN7
```

```text
Block 2

Replica 1 → DN2

Replica 2 → DN5

Replica 3 → DN8
```

---

# Advantages of Replication

## Fault Tolerance

Data survives node failures.

---

## High Availability

Data remains accessible.

---

## Reliability

Multiple copies reduce risk of data loss.

---

## Better Read Performance

Clients can read from nearest replica.

---

# Replica Placement Strategy

HDFS does not place all replicas on the same machine.

Why?

Because a single machine failure would destroy all copies.

---

# Default Replica Placement

Typically:

```text
Replica 1 → Same Rack

Replica 2 → Different Rack

Replica 3 → Same Rack as Replica 2
```

Example:

```text
Rack 1

DN1
DN2

Rack 2

DN5
DN6
```

Placement:

```text
Copy 1 → DN1

Copy 2 → DN5

Copy 3 → DN6
```

---

# Why Different Racks?

Protection against:

* Server Failure
* Switch Failure
* Rack Failure
* Power Failure

---

# Under-Replicated Blocks

## What Are Under-Replicated Blocks?

Blocks having fewer replicas than required.

Example:

Desired:

```text
Replication Factor = 3
```

Current:

```text
Only 2 Copies Exist
```

Result:

```text
Under-Replicated Block
```

---

# Causes

* DataNode Failure
* Network Failure
* Disk Failure

---

# Detection

Command:

```bash
hdfs fsck /
```

---

# Solution

NameNode automatically creates new replicas.

---

# Over-Replicated Blocks

## What Are Over-Replicated Blocks?

Blocks having more replicas than required.

Example:

Desired:

```text
3 Copies
```

Actual:

```text
5 Copies
```

---

# Causes

* Cluster Recovery
* Replication Misconfiguration

---

# Solution

NameNode removes extra replicas automatically.

---

# Re-Replication Process

Suppose:

```text
Block A

DN1
DN5
DN8
```

DN5 fails.

Current:

```text
DN1
DN8
```

Replication:

```text
2 Copies
```

Required:

```text
3 Copies
```

---

# NameNode Action

Select new DataNode:

```text
DN10
```

Create new replica:

```text
DN1

DN8

DN10
```

Replication restored.

---

# Replication During Write Operation

When a client uploads data:

## Step 1

Client contacts NameNode.

---

## Step 2

NameNode selects DataNodes.

---

## Step 3

Data written to first DataNode.

---

## Step 4

Pipeline replication begins.

Example:

```text
Client

↓

DN1

↓

DN4

↓

DN7
```

---

## Step 5

Replication completed.

---

# Changing Replication Factor

## Command

```bash
hdfs dfs -setrep 2 /data/file.csv
```

---

## Verify

```bash
hdfs dfs -ls /data
```

---

# Increasing Replication

```bash
hdfs dfs -setrep 5 /data/file.csv
```

NameNode creates additional replicas.

---

# Decreasing Replication

```bash
hdfs dfs -setrep 2 /data/file.csv
```

Extra replicas removed.

---

# Real-World Example

Suppose Netflix stores:

```text
100 TB Data
```

Replication Factor:

```text
3
```

Actual Storage Required:

```text
300 TB
```

Why?

Every block has three copies.

---

# Trade-Off

## Higher Replication

Advantages:

* Better Fault Tolerance
* Better Availability

Disadvantages:

* More Storage Consumption

---

## Lower Replication

Advantages:

* Less Storage Usage

Disadvantages:

* Higher Risk of Data Loss

---

# Best Practices

## Use Default Replication

```text
3
```

for most workloads.

---

## Monitor Under-Replicated Blocks

```bash
hdfs fsck /
```

---

## Monitor Cluster Health

```bash
hdfs dfsadmin -report
```

---

## Use Rack Awareness

Improves reliability.

---

# Common Mistakes

## Mistake 1

Replication Factor = 1

Result:

```text
No Fault Tolerance
```

---

## Mistake 2

Replication Factor = 10

Result:

```text
Storage Waste
```

---

## Mistake 3

Ignoring Dead DataNodes

Can create many under-replicated blocks.

---

# Interview Questions

## Beginner

1. What is replication in HDFS?
2. Why is replication needed?
3. What is the default replication factor?

---

## Intermediate

4. What are under-replicated blocks?
5. What are over-replicated blocks?
6. How does HDFS place replicas?

---

## Advanced

7. Explain the re-replication process.
8. What happens when a DataNode fails?
9. Why are replicas placed across racks?
10. How does replication improve fault tolerance?

---

# Quick Revision

### Replication

Multiple copies of blocks

### Default Replication Factor

3

### Purpose

Fault Tolerance

### Under-Replicated Block

Less replicas than required

### Over-Replicated Block

More replicas than required

### Re-Replication Trigger

DataNode Failure

### Command

```bash
hdfs dfs -setrep <factor> <file>
```

---

# Key Takeaways

* Replication is the foundation of HDFS fault tolerance.
* Default replication factor is 3.
* Replicas are distributed across DataNodes and racks.
* Under-replicated blocks are automatically repaired.
* Replication improves availability and reliability.
* Proper replication settings balance storage and fault tolerance.
