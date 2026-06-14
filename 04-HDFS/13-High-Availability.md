# HDFS High Availability (HA)

## Introduction

In traditional Hadoop architecture, a single NameNode manages the entire HDFS namespace.

This creates a major problem:

```text id="o3z4va"
Single Point of Failure (SPOF)
```

If the NameNode crashes:

* HDFS becomes inaccessible
* Clients cannot read data
* Clients cannot write data
* Cluster operations stop

To solve this problem, Hadoop introduced:

> **High Availability (HA)**

High Availability ensures HDFS remains available even when a NameNode fails.

---

# What is High Availability?

High Availability (HA) is a mechanism that eliminates the Single Point of Failure in HDFS by maintaining multiple NameNodes.

Typically:

```text id="hbh8tz"
Active NameNode

+

Standby NameNode
```

If Active NameNode fails:

```text id="8n1q7i"
Standby NameNode

↓

Becomes Active
```

Cluster continues operating.

---

# Why Do We Need HA?

Traditional Architecture:

```text id="wq1nkr"
           NameNode
               |
      ------------------
      |        |       |
     DN1      DN2     DN3
```

---

# Problem

If NameNode crashes:

```text id="x7l3yq"
Metadata Unavailable
```

Result:

* No file access
* No block location lookup
* Cluster outage

---

# HA Architecture

```text id="h4n2wq"
           Active NN
                |
                |
           Shared Edits
                |
                |
          Standby NN

                |
      ----------------------
      |         |          |

     DN1       DN2        DN3
```

Both NameNodes maintain identical metadata.

---

# Components of HDFS HA

## Active NameNode

Primary NameNode handling all client requests.

Responsibilities:

* Read Operations
* Write Operations
* Metadata Management

---

## Standby NameNode

Backup NameNode.

Responsibilities:

* Continuously synchronize metadata
* Monitor Active NameNode
* Take over during failure

---

## JournalNodes

Store shared edit logs.

Purpose:

Keep Active and Standby NameNodes synchronized.

---

## ZooKeeper

Coordinates failover process.

Helps determine:

```text id="t9g1zz"
Which NameNode Should Be Active
```

---

# HA Architecture Diagram

```text id="d2u0lc"
                  ZooKeeper

                      |

       -------------------------------

       |                             |

       v                             v

 Active NameNode          Standby NameNode

       |                             |

       -------- JournalNodes --------

                     |

         -------------------------

         |          |           |

        DN1        DN2         DN3
```

---

# Role of JournalNodes

JournalNodes store:

```text id="6o3bwl"
Edit Logs
```

When Active NameNode receives changes:

Example:

```text id="1u9d3x"
Create File

Delete File

Rename File
```

These edits are written to JournalNodes.

Standby NameNode reads them continuously.

---

# Metadata Synchronization

Example:

Active NameNode:

```text id="r4d5st"
/finance

/hr

/sales
```

Standby NameNode receives identical updates.

Both remain synchronized.

---

# Failover Process

## Step 1

Active NameNode crashes.

---

## Step 2

ZooKeeper detects failure.

---

## Step 3

Standby NameNode promoted.

---

## Step 4

Standby becomes Active.

---

## Step 5

Clients reconnect automatically.

---

# Failover Workflow

```text id="4tqv0z"
Active NN

↓

Failure

↓

ZooKeeper Detects

↓

Standby NN

↓

Active NN
```

---

# Types of Failover

## Manual Failover

Administrator performs failover.

Command:

```bash id="b0p6hl"
hdfs haadmin -failover nn1 nn2
```

---

## Automatic Failover

ZooKeeper performs failover automatically.

Most production clusters use this approach.

---

# ZooKeeper's Role

ZooKeeper manages:

* Leader Election
* Failure Detection
* Coordination

Purpose:

Prevent:

```text id="k4r4vh"
Split Brain
```

situation.

---

# What is Split Brain?

Dangerous scenario:

```text id="l4zh1g"
NN1 Active

NN2 Active
```

Both NameNodes think they are active.

Result:

Metadata inconsistency.

ZooKeeper prevents this.

---

# Active vs Standby NameNode

| Feature                 | Active NN | Standby NN |
| ----------------------- | --------- | ---------- |
| Handles Client Requests | Yes       | No         |
| Metadata Updates        | Yes       | No         |
| Reads Edit Logs         | No        | Yes        |
| Failover Candidate      | No        | Yes        |
| Cluster Operations      | Yes       | No         |

---

# JournalNodes Architecture

Production setup usually uses:

```text id="8vwnqi"
3 JournalNodes
```

or

```text id="b5t7vr"
5 JournalNodes
```

Example:

```text id="c8xq7u"
JN1

JN2

JN3
```

Majority required for consensus.

---

# HA vs Federation

Very common interview question.

| Feature            | HA               | Federation           |
| ------------------ | ---------------- | -------------------- |
| Purpose            | Fault Tolerance  | Scalability          |
| NameNodes          | Active + Standby | Multiple Independent |
| Solves             | SPOF             | Metadata Bottleneck  |
| Automatic Failover | Yes              | No                   |
| Namespace Scaling  | No               | Yes                  |

---

# Federation + HA

Large clusters use both.

Example:

```text id="y3hv7s"
Finance Namespace

Active NN

Standby NN
```

```text id="1jgrj0"
HR Namespace

Active NN

Standby NN
```

Provides:

* Scalability
* Fault Tolerance

---

# Real-World Example

Suppose a bank operates:

```text id="sj1v40"
500 TB Data
```

and

```text id="krzz6f"
Millions of Transactions
```

If NameNode fails:

System outage becomes unacceptable.

HA ensures:

```text id="52bmcc"
Standby NN

↓

Automatically Takes Over
```

No downtime.

---

# Benefits of HA

## Eliminates Single Point of Failure

Most important benefit.

---

## Improved Reliability

Continuous cluster availability.

---

## Faster Recovery

Automatic failover.

---

## Enterprise Readiness

Suitable for production environments.

---

# Limitations

## More Components

Need:

* ZooKeeper
* JournalNodes
* Additional NameNodes

---

## More Configuration

HA setup is more complex.

---

## Additional Hardware

Requires extra resources.

---

# Common Mistakes

## Mistake 1

Confusing HA with Federation.

HA:

```text id="y39h7g"
Fault Tolerance
```

Federation:

```text id="odmw9s"
Scalability
```

---

## Mistake 2

Assuming Secondary NameNode provides HA.

Reality:

It only performs checkpointing.

---

## Mistake 3

Ignoring ZooKeeper.

ZooKeeper is critical for failover.

---

# Interview Scenario

## Scenario

Active NameNode crashes unexpectedly.

Question:

What happens in an HA cluster?

Answer:

1. ZooKeeper detects failure.
2. Standby NameNode promoted.
3. Clients reconnect.
4. Cluster continues operating.

---

# Interview Questions

## Beginner

1. What is HDFS High Availability?
2. Why is HA needed?

---

## Intermediate

3. Difference between Active and Standby NameNode?
4. What are JournalNodes?
5. What is ZooKeeper's role?

---

## Advanced

6. Explain failover process.
7. What is Split Brain?
8. Difference between HA and Federation?
9. How do JournalNodes maintain consistency?
10. Explain complete HA architecture.

---

# Quick Revision

### HA

High Availability

### Purpose

Remove Single Point of Failure

### Components

* Active NameNode
* Standby NameNode
* ZooKeeper
* JournalNodes

### Failover

Automatic or Manual

### ZooKeeper Role

Coordination

### JournalNode Role

Shared Edit Logs

### Solves

NameNode Failure

### Provides Scalability?

No

### Provides Fault Tolerance?

Yes

---

# Key Takeaways

* High Availability eliminates the NameNode Single Point of Failure.
* HA uses Active and Standby NameNodes.
* JournalNodes synchronize metadata changes.
* ZooKeeper manages failover and coordination.
* Automatic failover minimizes downtime.
* HA improves reliability and availability in production clusters.
* HA and Federation solve different problems and are often used together.
