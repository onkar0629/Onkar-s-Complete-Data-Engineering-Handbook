# Hadoop Architecture

## Introduction

Hadoop Architecture is based on a Master-Slave architecture that enables distributed storage and distributed processing of large datasets across multiple machines.

The architecture is designed to provide:

* Scalability
* Fault Tolerance
* High Availability
* Cost Efficiency

A Hadoop cluster consists of multiple interconnected machines working together to store and process data.

---

# Components of Hadoop Architecture

The core components are:

1. HDFS (Storage Layer)
2. YARN (Resource Management Layer)
3. MapReduce (Processing Layer)

---

# High-Level Architecture

```text
                    Client
                       |
                       v
      ---------------------------------
      |                               |
      v                               v

   NameNode                   ResourceManager
   (HDFS Master)              (YARN Master)

      |                               |
      |                               |
      ---------------------------------
                       |
       -----------------------------------------
       |                  |                  |
       v                  v                  v

   DataNode          DataNode          DataNode
(NodeManager)     (NodeManager)     (NodeManager)

```

---

# HDFS Architecture

HDFS follows a Master-Slave model.

## NameNode (Master)

The NameNode is responsible for managing the file system metadata.

### Responsibilities

* Stores file metadata
* Maintains directory structure
* Tracks block locations
* Handles client requests

### Metadata Examples

* File Name
* File Permissions
* Block Locations
* Replication Information

### Important Point

NameNode does NOT store actual data.

It only stores metadata.

---

## DataNode (Worker)

DataNodes store the actual data blocks.

### Responsibilities

* Store data blocks
* Serve read/write requests
* Send heartbeat messages
* Send block reports

### Important Point

Actual data resides in DataNodes.

---

# YARN Architecture

YARN manages cluster resources and job execution.

---

## ResourceManager (Master)

The ResourceManager manages resources across the cluster.

### Responsibilities

* Resource allocation
* Job scheduling
* Cluster management

Only one ResourceManager exists in a cluster.

---

## NodeManager (Worker)

Runs on every worker node.

### Responsibilities

* Monitor resources
* Manage containers
* Communicate with ResourceManager

---

## ApplicationMaster

Created for every application/job.

### Responsibilities

* Negotiate resources
* Coordinate execution
* Monitor task progress

---

# MapReduce Architecture

MapReduce is the processing engine of Hadoop.

## Components

### Mapper

Processes input data.

Produces key-value pairs.

### Shuffle and Sort

Groups similar keys together.

### Reducer

Processes grouped data and generates final output.

---

# Data Flow During File Write

Suppose a user uploads a file to HDFS.

## Step 1

Client sends request to NameNode.

---

## Step 2

NameNode identifies suitable DataNodes.

---

## Step 3

Client sends blocks to DataNodes.

---

## Step 4

Blocks are replicated.

Example:

Replication Factor = 3

```text
Block A

DataNode 1
DataNode 4
DataNode 7
```

---

## Step 5

NameNode updates metadata.

File successfully stored.

---

# Data Flow During File Read

## Step 1

Client requests file.

---

## Step 2

NameNode returns block locations.

---

## Step 3

Client directly contacts DataNodes.

---

## Step 4

Data is retrieved.

---

# Heartbeat Mechanism

Every DataNode periodically sends a heartbeat to the NameNode.

Purpose:

* Indicates node is alive
* Helps detect failures

Default heartbeat interval:

Approximately 3 seconds.

---

# Block Report

DataNodes periodically send block reports.

Contains:

* List of stored blocks
* Block status information

Purpose:

Allows NameNode to maintain accurate metadata.

---

# Fault Tolerance

Hadoop achieves fault tolerance through replication.

Example:

Replication Factor = 3

```text
Block A

Copy 1 -> DataNode 1
Copy 2 -> DataNode 5
Copy 3 -> DataNode 8
```

If DataNode 5 fails:

* Data remains available
* Replication is automatically restored

---

# Rack Awareness

Hadoop distributes replicas across multiple racks.

Benefits:

* Better fault tolerance
* Protection against rack failures
* Improved network performance

---

# Data Locality

Hadoop moves computation closer to data.

Instead of:

```text
Move Data -> Processing
```

Hadoop uses:

```text
Move Processing -> Data
```

Benefits:

* Reduced network traffic
* Faster execution

---

# Real-World Example

Netflix stores petabytes of user activity data.

Data is:

* Distributed across many DataNodes
* Processed using parallel computation
* Managed using YARN

This enables efficient large-scale analytics.

---

# Common Interview Questions

## Beginner

1. Explain Hadoop Architecture.
2. What is the role of NameNode?
3. What is the role of DataNode?

## Intermediate

4. Explain HDFS architecture.
5. Explain YARN architecture.
6. What is a heartbeat?

## Advanced

7. What happens when a DataNode fails?
8. Explain rack awareness.
9. Explain data locality.
10. How does Hadoop achieve fault tolerance?

---

# Key Takeaways

* Hadoop follows a Master-Slave architecture.
* HDFS provides distributed storage.
* YARN manages resources and job execution.
* MapReduce processes data in parallel.
* NameNode stores metadata.
* DataNodes store actual data.
* Fault tolerance is achieved through replication.
* Data locality improves performance by moving computation closer to data.
