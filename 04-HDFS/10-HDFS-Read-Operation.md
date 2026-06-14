# HDFS Read Operation

## Introduction

The HDFS Read Operation describes how a client retrieves data from HDFS.

Although files are physically stored as blocks across multiple DataNodes, HDFS makes the process appear seamless to the user.

One of the key design goals of HDFS read operations is:

* High Throughput
* Fault Tolerance
* Data Locality
* Efficient Network Usage

---

# What Happens During a Read Operation?

When a client requests a file:

```text
sales.csv
```

HDFS must:

1. Identify where file blocks are stored.
2. Find the nearest replica.
3. Retrieve data from DataNodes.
4. Return the file to the client.

---

# Components Involved

## Client

User or application requesting data.

---

## NameNode

Provides metadata.

Specifically:

* Block locations
* Replica locations

---

## DataNodes

Store actual blocks.

Provide block data to the client.

---

# High-Level Workflow

```text
          Client
             |
             |
             v
        NameNode
             |
             |
             v
      Block Locations
             |
             |
             v
        DataNodes
             |
             |
             v
          Client
```

---

# Step-by-Step Read Operation

Suppose:

```text
sales.csv
```

Size:

```text
300 MB
```

Block Size:

```text
128 MB
```

Blocks:

```text
Block A

Block B

Block C
```

---

# Step 1: Client Requests File

Client sends request:

```text
Read sales.csv
```

to NameNode.

---

# Step 2: NameNode Checks Metadata

NameNode looks up metadata.

Example:

```text
sales.csv

Block A → DN1

Block B → DN2

Block C → DN3
```

---

# Step 3: NameNode Returns Block Locations

NameNode sends:

```text
Block A → DN1

Block B → DN2

Block C → DN3
```

to client.

---

# Important Point

NameNode does NOT send actual data.

Only metadata.

---

# Step 4: Client Contacts DataNodes

Client directly communicates with DataNodes.

Example:

```text
Client

↓

DN1

↓

Block A
```

---

# Step 5: Data Transfer Begins

DataNodes stream blocks to client.

Example:

```text
DN1 → Block A

DN2 → Block B

DN3 → Block C
```

---

# Step 6: Client Reconstructs File

Client combines:

```text
Block A

+

Block B

+

Block C
```

Final Output:

```text
sales.csv
```

---

# Read Operation Architecture

```text
                    Client
                       |
                       |
                       v

                  NameNode

                       |

       ----------------------------------

       |               |               |

       v               v               v

      DN1             DN2             DN3

   Block A         Block B         Block C

       |               |               |

       ----------------------------------

                       |

                       v

                    Client
```

---

# Replica Selection During Read

Suppose:

```text
Block A
```

has replicas:

```text
DN1

DN5

DN8
```

HDFS chooses:

### 1st Preference

Nearest Replica

---

### 2nd Preference

Same Rack Replica

---

### 3rd Preference

Remote Rack Replica

---

# Why?

To reduce:

* Network Traffic
* Latency
* Bandwidth Usage

---

# Data Locality During Read

Suppose:

```text
Client
```

is on:

```text
DN5
```

and Block A exists on:

```text
DN5
```

HDFS reads locally.

Benefits:

* Fastest access
* No network transfer

---

# Fault Tolerance During Read

Suppose:

```text
Block A
```

stored on:

```text
DN1

DN5

DN8
```

DN1 fails.

---

# What Happens?

HDFS automatically reads from:

```text
DN5
```

or

```text
DN8
```

No user intervention required.

---

# Example

File:

```text
1 GB
```

Block Size:

```text
128 MB
```

Blocks:

```text
8 Blocks
```

Client receives:

```text
Block 1

Block 2

Block 3

...

Block 8
```

from different DataNodes.

---

# Why Read Operation is Fast

## Parallel Access

Blocks can be read simultaneously.

---

## Data Locality

Nearest replica chosen.

---

## Replication

Multiple copies available.

---

## Streaming Access

Optimized for large files.

---

# Real-World Example

Netflix stores:

```text
10 TB Viewing Data
```

across hundreds of DataNodes.

When analytics jobs read data:

* Nearest replicas selected
* Multiple blocks read in parallel
* Network traffic minimized

Result:

Fast processing of huge datasets.

---

# Read Operation vs Traditional File System

| Traditional File System | HDFS                  |
| ----------------------- | --------------------- |
| Single Server           | Multiple DataNodes    |
| Single Disk Access      | Distributed Access    |
| Limited Scalability     | Highly Scalable       |
| No Replication          | Replication Supported |
| Single Failure Risk     | Fault Tolerant        |

---

# Common Mistakes

## Mistake 1

Thinking NameNode sends data.

Reality:

NameNode only provides metadata.

---

## Mistake 2

Assuming data comes from one DataNode.

Reality:

Different blocks can come from different DataNodes.

---

## Mistake 3

Ignoring replica selection.

Nearest replica is preferred.

---

# Interview Scenario

## Scenario

Block A exists on:

```text
DN1

DN5

DN8
```

DN1 fails during read operation.

Question:

Will the file become unavailable?

Answer:

No.

HDFS reads from another replica.

---

# Interview Questions

## Beginner

1. Explain HDFS Read Operation.
2. Which component stores metadata?
3. Which component stores actual data?

---

## Intermediate

4. Why does the client contact DataNodes directly?
5. How does HDFS select a replica?

---

## Advanced

6. Explain the complete read workflow.
7. What happens if a DataNode fails during read?
8. How does Data Locality improve read performance?
9. Why is NameNode not involved in data transfer?
10. How does replication improve read availability?

---

# Quick Revision

### Step 1

Client contacts NameNode

### Step 2

NameNode returns block locations

### Step 3

Client contacts DataNodes

### Step 4

Data transferred

### Step 5

Client reconstructs file

### NameNode Sends

Metadata

### DataNode Sends

Actual Data

### Read Optimization

Nearest Replica

### Fault Tolerance

Read from another replica

---

# Key Takeaways

* HDFS Read Operation starts with the NameNode and ends with DataNodes serving data.
* NameNode provides metadata only.
* DataNodes provide actual block data.
* Clients communicate directly with DataNodes.
* Replica selection improves performance.
* Replication ensures availability during failures.
* Data Locality reduces network overhead and improves throughput.
