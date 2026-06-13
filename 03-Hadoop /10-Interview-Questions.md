# Hadoop Interview Questions

## Introduction

This chapter contains frequently asked Hadoop interview questions categorized by difficulty level.

These questions are commonly asked in:

* Data Engineer Interviews
* Big Data Engineer Interviews
* Hadoop Administrator Interviews
* Fresher Interviews

---

# Beginner Level Questions

## 1. What is Hadoop?

Hadoop is an open-source framework used for distributed storage and distributed processing of large datasets across clusters of computers.

---

## 2. Why was Hadoop developed?

Hadoop was developed to solve Big Data challenges such as:

* Large-scale storage
* Distributed processing
* Scalability
* Fault tolerance

---

## 3. What are the core components of Hadoop?

* HDFS
* YARN
* MapReduce

---

## 4. What is HDFS?

HDFS (Hadoop Distributed File System) is a distributed storage system used to store large files across multiple machines.

---

## 5. What is YARN?

YARN (Yet Another Resource Negotiator) is responsible for resource management and job scheduling.

---

## 6. What is MapReduce?

MapReduce is a programming model used for distributed processing of large datasets.

---

## 7. What is a Hadoop Cluster?

A collection of interconnected machines working together for distributed storage and processing.

---

## 8. What is Big Data?

Data that is too large and complex to be processed by traditional systems.

---

## 9. What are the 3 Vs of Big Data?

* Volume
* Velocity
* Variety

---

## 10. What is Fault Tolerance?

The ability of Hadoop to continue functioning even when nodes fail.

---

# Intermediate Level Questions

## 11. What is the default block size in Hadoop?

128 MB (commonly in Hadoop 2.x and later).

---

## 12. What is the default replication factor?

3

---

## 13. What is NameNode?

The master node that stores metadata.

---

## 14. What is DataNode?

Worker node that stores actual data blocks.

---

## 15. What is Secondary NameNode?

A helper service that merges FsImage and Edit Logs.

It is NOT a backup NameNode.

---

## 16. What is Data Locality?

Executing computation where data resides to reduce network traffic.

---

## 17. What is Rack Awareness?

Placing replicas across different racks to improve fault tolerance.

---

## 18. What is a Heartbeat?

A signal sent by DataNodes to NameNode indicating they are alive.

---

## 19. What is a Block Report?

A report containing information about blocks stored on a DataNode.

---

## 20. What is Safe Mode?

A read-only mode used by NameNode during startup.

---

# Advanced Level Questions

## 21. Explain Hadoop Architecture.

Hadoop follows a Master-Slave architecture consisting of:

Master:

* NameNode
* ResourceManager

Workers:

* DataNodes
* NodeManagers

---

## 22. How does Hadoop achieve fault tolerance?

Through block replication.

---

## 23. What happens when a DataNode fails?

1. Heartbeat stops.
2. NameNode detects failure.
3. Blocks become under-replicated.
4. Replication is restored.

---

## 24. Why does Hadoop use large block sizes?

To reduce disk seek operations and improve throughput.

---

## 25. Explain HDFS Write Operation.

1. Client contacts NameNode.
2. NameNode returns DataNode locations.
3. Client writes blocks.
4. Replication occurs.
5. Metadata updated.

---

## 26. Explain HDFS Read Operation.

1. Client requests file.
2. NameNode provides block locations.
3. Client reads from DataNodes.

---

## 27. Why is Hadoop horizontally scalable?

New machines can be added without changing existing infrastructure.

---

## 28. What is the Small Files Problem?

Millions of small files consume excessive NameNode memory.

---

## 29. How can the Small Files Problem be solved?

Use:

* Parquet
* ORC
* Avro
* Sequence Files

---

## 30. Difference Between Vertical and Horizontal Scaling?

| Vertical Scaling | Horizontal Scaling |
| ---------------- | ------------------ |
| Upgrade server   | Add servers        |
| Expensive        | Cost Effective     |
| Limited          | Highly Scalable    |

---

# Scenario-Based Questions

## 31. A DataNode crashes. What happens?

NameNode detects heartbeat failure and initiates re-replication.

---

## 32. A file cannot be uploaded because NameNode is in Safe Mode. What will you do?

Check:

```bash
hdfs dfsadmin -safemode get
```

Leave Safe Mode:

```bash
hdfs dfsadmin -safemode leave
```

---

## 33. Cluster performance is very slow. What could be the reasons?

Possible causes:

* Small files
* Network bottlenecks
* Low memory
* High replication factor
* Data skew

---

## 34. How do you check Hadoop cluster health?

```bash
hdfs dfsadmin -report
```

---

## 35. How do you check running Hadoop services?

```bash
jps
```

---

# Frequently Asked Fresher Questions

## 36. Why do you want to work with Hadoop?

Focus on:

* Scalability
* Big Data Processing
* Distributed Systems

---

## 37. Which Hadoop components have you worked with?

Example:

* HDFS
* YARN
* Hive
* Sqoop
* MapReduce

---

## 38. Explain a project where you used Hadoop.

Prepare at least one project explanation.

---

## 39. Why is Hadoop preferred over traditional systems?

Because it provides:

* Distributed storage
* Fault tolerance
* Horizontal scalability

---

## 40. What is the role of YARN?

Resource management and job scheduling.

---

# Quick Revision Questions

1. Default Block Size?
   → 128 MB

2. Default Replication Factor?
   → 3

3. NameNode Stores?
   → Metadata

4. DataNode Stores?
   → Actual Data

5. Heartbeat Interval?
   → Approximately 3 seconds

6. Block Report?
   → Information about stored blocks

7. Safe Mode?
   → Read-only NameNode state

8. Hadoop Scaling?
   → Horizontal

9. Processing Framework?
   → MapReduce

10. Resource Manager?
    → YARN

---

# Top 10 Questions Most Frequently Asked

1. What is Hadoop?
2. Explain Hadoop Architecture.
3. Difference between NameNode and DataNode.
4. What is HDFS?
5. What is YARN?
6. What is Data Locality?
7. What is Rack Awareness?
8. What happens when a DataNode fails?
9. What is the Small Files Problem?
10. Explain Fault Tolerance in Hadoop.

---

# Interview Preparation Tips

✅ Understand architecture thoroughly.

✅ Learn HDFS read and write operations.

✅ Know block size and replication.

✅ Practice HDFS commands.

✅ Understand Data Locality and Rack Awareness.

✅ Prepare at least one Hadoop project explanation.

✅ Be ready for scenario-based questions.

---

# Key Takeaways

* Hadoop interviews focus heavily on architecture.
* HDFS concepts are frequently asked.
* Scenario-based questions are becoming more common.
* Understanding failures and troubleshooting gives an advantage.
* Practical experience with commands is important.
