# Rack Awareness

## Introduction

Rack Awareness is an HDFS feature that determines how replicas are distributed across the cluster.

It helps Hadoop make intelligent decisions about:

* Replica Placement
* Fault Tolerance
* Network Traffic
* Data Availability

Without Rack Awareness, all replicas might end up in the same rack, creating a risk of data loss during rack failures.

---

# What is a Rack?

A rack is a physical structure that contains multiple servers connected through a common network switch.

Example:

```text
Rack 1
│
├── DataNode 1
├── DataNode 2
├── DataNode 3
└── Switch 1
```

```text
Rack 2
│
├── DataNode 4
├── DataNode 5
├── DataNode 6
└── Switch 2
```

A Hadoop cluster may contain multiple racks.

---

# Why Do We Need Rack Awareness?

Consider a cluster:

```text
Rack 1

DN1
DN2
DN3
```

Replication Factor:

```text
3
```

Suppose all replicas are stored in Rack 1:

```text
Block A

DN1
DN2
DN3
```

---

# Problem

If Rack 1 fails:

* Switch Failure
* Power Failure
* Network Failure

Result:

```text
All Replicas Lost
```

Potential data unavailability.

---

# Solution

Store replicas across different racks.

Example:

```text
Block A

DN1 (Rack 1)

DN5 (Rack 2)

DN6 (Rack 2)
```

Now a rack failure will not destroy all replicas.

---

# Rack Awareness Architecture

```text
                NameNode
                    |
       --------------------------
       |                        |
       v                        v

      Rack 1                 Rack 2

   DN1  DN2  DN3         DN4  DN5  DN6
```

The NameNode understands rack topology and uses it while placing replicas.

---

# Default Replica Placement Strategy

Replication Factor:

```text
3
```

HDFS generally places replicas as:

### Replica 1

Same node where write occurs (or nearest node).

---

### Replica 2

Different rack.

---

### Replica 3

Same rack as Replica 2 but on a different node.

---

# Example

```text
Rack 1

DN1
DN2
DN3
```

```text
Rack 2

DN4
DN5
DN6
```

Placement:

```text
Block A

Replica 1 → DN1

Replica 2 → DN5

Replica 3 → DN6
```

---

# Why This Strategy?

Goals:

* Fault Tolerance
* Network Efficiency
* Better Availability

---

# Rack Failure Scenario

Suppose:

```text
Block A

DN1 (Rack 1)

DN5 (Rack 2)

DN6 (Rack 2)
```

Rack 1 fails.

Result:

```text
DN1 Lost
```

Still Available:

```text
DN5

DN6
```

No data loss.

---

# Network Topology Awareness

NameNode maintains information about:

```text
Rack 1

Rack 2

Rack 3
```

and knows:

```text
Which DataNode belongs to which Rack
```

This helps in intelligent replica placement.

---

# Impact on Write Operations

During file upload:

### Step 1

Client contacts NameNode.

---

### Step 2

NameNode selects DataNodes across racks.

---

### Step 3

Pipeline created.

Example:

```text
Client

↓

DN1

↓

DN5

↓

DN6
```

---

### Step 4

Replication completed.

---

# Impact on Read Operations

When reading data:

NameNode returns multiple replica locations.

Client prefers:

## Local Replica

Same machine.

---

## Same Rack Replica

Second preference.

---

## Different Rack Replica

Last preference.

This improves performance.

---

# Rack Awareness and Data Locality

Rack Awareness works closely with Data Locality.

Goal:

```text
Move Processing To Data
```

instead of:

```text
Move Data To Processing
```

Benefits:

* Lower Network Traffic
* Faster Execution

---

# Rack Awareness Configuration

Administrators provide:

```text
Rack Topology Script
```

which tells Hadoop:

```text
DN1 → Rack 1

DN2 → Rack 1

DN3 → Rack 2
```

NameNode uses this mapping.

---

# Real-World Example

Suppose Netflix operates:

```text
500 DataNodes
```

Distributed across:

```text
20 Racks
```

Rack Awareness ensures:

* Replicas spread across racks
* Rack failures do not affect availability
* Better cluster reliability

---

# Advantages of Rack Awareness

## Improved Fault Tolerance

Protects against rack failures.

---

## Better Availability

Multiple replicas remain accessible.

---

## Reduced Data Loss Risk

Replica distribution improves safety.

---

## Optimized Network Usage

Reduces unnecessary network traffic.

---

# Limitations

## Additional Configuration

Requires rack topology setup.

---

## Network Overhead During Writes

Cross-rack replication consumes bandwidth.

---

## Complex Large Cluster Management

Topology maintenance becomes important.

---

# Common Mistakes

## Mistake 1

Assuming replication alone is enough.

Reality:

Rack failures can still occur.

---

## Mistake 2

Keeping all replicas in one rack.

Creates a single point of failure.

---

## Mistake 3

Ignoring network topology.

Can lead to poor performance.

---

# Interview Scenario

## Scenario

Replication Factor:

```text
3
```

All replicas stored in Rack 1.

Rack 1 loses power.

Question:

What happens?

Answer:

All replicas become unavailable.

This is why Rack Awareness is needed.

---

# Interview Questions

## Beginner

1. What is a rack?
2. What is Rack Awareness?
3. Why is Rack Awareness needed?

---

## Intermediate

4. How does Hadoop know rack locations?
5. Explain replica placement strategy.
6. How does Rack Awareness improve reliability?

---

## Advanced

7. What happens during a rack failure?
8. How does Rack Awareness affect read and write operations?
9. Explain Rack Awareness architecture.
10. Difference between Replication and Rack Awareness?

---

# Quick Revision

### Rack

Collection of servers connected through a switch

### Purpose

Improve fault tolerance

### Managed By

NameNode

### Replica Placement

Across multiple racks

### Benefits

* Fault Tolerance
* High Availability
* Better Network Usage

### Protects Against

* Switch Failure
* Power Failure
* Rack Failure

### Works With

Replication

---

# Key Takeaways

* Rack Awareness determines where replicas are stored.
* Hadoop distributes replicas across different racks.
* It protects against rack-level failures.
* NameNode uses rack topology information for replica placement.
* Read and write operations benefit from Rack Awareness.
* It improves reliability, availability, and fault tolerance in large Hadoop clusters.
