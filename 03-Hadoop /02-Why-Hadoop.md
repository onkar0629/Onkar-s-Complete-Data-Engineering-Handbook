# Why Hadoop?

## Introduction

Before Hadoop, organizations primarily relied on traditional databases and single-server systems to store and process data.

As data volumes increased rapidly due to the internet, social media, mobile devices, and IoT, traditional systems struggled to handle the scale efficiently.

Hadoop was introduced to solve these challenges through distributed storage and distributed processing.

---

# The Problem Before Hadoop

Traditional systems worked well when:

* Data size was small
* Number of users was limited
* Data growth was predictable

However, modern organizations started generating terabytes and petabytes of data.

Examples:

* Facebook user activities
* Netflix viewing history
* Amazon purchase records
* Banking transactions
* Sensor and IoT data

Traditional systems became expensive and inefficient at this scale.

---

# Challenges of Traditional Systems

## 1. Limited Storage Capacity

Traditional systems store data on a single server.

As data grows:

* Disk space becomes insufficient
* Upgrading storage becomes expensive

### Example

A company generates 500 TB of data.

A single server cannot efficiently store and manage this amount of data.

---

## 2. Slow Processing

Processing large datasets on one machine requires significant time.

### Example

Analyzing billions of records on a single server may take hours or days.

---

## 3. Single Point of Failure

If the server crashes:

* Data becomes unavailable
* Applications stop functioning

This creates reliability issues.

---

## 4. High Infrastructure Cost

Traditional systems scale vertically.

### Vertical Scaling

Increasing resources of a single machine:

* More CPU
* More RAM
* Larger Storage

However:

* Hardware becomes expensive
* There is a physical limit to upgrades

---

# Hadoop's Solution

Hadoop introduced:

## Distributed Storage

Store data across multiple machines.

Instead of:

```text
1 Server = 100 TB
```

Use:

```text
10 Servers = 10 TB each
```

Data is distributed across the cluster.

---

## Distributed Processing

Instead of one machine processing data:

```text
1 Machine → Entire Workload
```

Hadoop uses:

```text
100 Machines → Shared Workload
```

Multiple machines process data simultaneously.

---

## Fault Tolerance

Hadoop stores multiple copies of data.

### Example

Replication Factor = 3

```text
Block A

DataNode 1
DataNode 5
DataNode 8
```

If one node fails:

* Other replicas remain available
* Processing continues

---

## Horizontal Scalability

Hadoop scales by adding more machines.

### Horizontal Scaling

```text
10 Nodes
20 Nodes
50 Nodes
100 Nodes
```

Advantages:

* Cost Effective
* Virtually Unlimited Growth

---

# Vertical Scaling vs Horizontal Scaling

| Feature         | Vertical Scaling        | Horizontal Scaling |
| --------------- | ----------------------- | ------------------ |
| Method          | Upgrade Existing Server | Add More Servers   |
| Cost            | High                    | Lower              |
| Limit           | Hardware Limit          | Easily Expandable  |
| Fault Tolerance | Low                     | High               |
| Hadoop Support  | No                      | Yes                |

---

# Traditional Database vs Hadoop

| Feature         | Traditional Database | Hadoop         |
| --------------- | -------------------- | -------------- |
| Data Size       | GB to TB             | TB to PB       |
| Scalability     | Limited              | Massive        |
| Cost            | Expensive            | Cost Effective |
| Fault Tolerance | Limited              | High           |
| Processing      | Single Machine       | Distributed    |
| Storage         | Centralized          | Distributed    |

---

# Real-World Example

## Netflix

Netflix generates enormous amounts of data:

* Viewing history
* Search activity
* User interactions
* Recommendations

A traditional system would struggle to process this data efficiently.

Hadoop enables:

* Distributed storage
* Parallel processing
* Cost-effective scaling

---

# Why Companies Use Hadoop

Organizations adopt Hadoop because it provides:

* Scalability
* Fault Tolerance
* Cost Efficiency
* High Availability
* Distributed Computing
* Big Data Processing Capability

---

# Interview Questions

### Beginner

1. Why was Hadoop developed?
2. What problems does Hadoop solve?
3. Why can't traditional databases handle Big Data efficiently?

### Intermediate

4. Explain vertical scaling and horizontal scaling.
5. Why is Hadoop cost-effective?

### Advanced

6. Why is horizontal scaling preferred in Hadoop?
7. How does Hadoop eliminate single points of failure?
8. Explain distributed storage and distributed processing.

---

# Key Takeaways

* Hadoop was created to address Big Data challenges.
* Traditional systems struggle with storage, processing, and scalability.
* Hadoop uses distributed storage and distributed processing.
* Hadoop scales horizontally by adding machines.
* Fault tolerance is achieved through data replication.
* Hadoop provides a cost-effective solution for large-scale data processing.
