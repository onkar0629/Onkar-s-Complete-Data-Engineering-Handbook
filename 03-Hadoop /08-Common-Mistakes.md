# Common Hadoop Mistakes

## Introduction

Hadoop is designed to handle massive datasets efficiently, but poor design decisions and incorrect configurations can severely impact performance, scalability, and cluster health.

This chapter covers common mistakes made by beginners and even experienced engineers.

Understanding these mistakes helps in:

* Better cluster management
* Improved performance
* Faster troubleshooting
* Interview preparation

---

# Mistake 1: Small Files Problem

## Problem

Storing millions of small files in HDFS.

Example:

```text
1,000,000 files
Each file = 10 KB
```

---

## Why It Is Bad

HDFS is optimized for large files.

Each file, directory, and block requires metadata in NameNode memory.

Millions of small files consume excessive memory.

---

## Impact

* NameNode memory pressure
* Slow cluster performance
* Metadata bottleneck

---

## Solution

Use:

* Parquet
* ORC
* Avro
* Sequence Files

Merge small files whenever possible.

---

## Interview Question

Why are small files considered a problem in HDFS?

---

# Mistake 2: Wrong Replication Factor

## Problem

Setting replication too low.

Example:

```text
Replication Factor = 1
```

---

## Impact

If a DataNode fails:

* Data loss occurs
* No backup copy exists

---

## Solution

Use:

```text
Replication Factor = 3
```

(Default in most clusters)

---

# Mistake 3: Excessive Replication

## Problem

Setting replication factor too high.

Example:

```text
Replication Factor = 10
```

---

## Impact

* Storage waste
* Increased network traffic
* Slower writes

---

## Solution

Use replication based on business requirements.

Typically:

```text
3
```

is sufficient.

---

# Mistake 4: Poor Directory Structure

## Problem

Dumping all files into one directory.

Example:

```text
/data
```

containing thousands of files.

---

## Impact

* Difficult management
* Poor organization
* Harder troubleshooting

---

## Better Structure

```text
/data
   /raw
   /processed
   /archive
```

---

# Mistake 5: Ignoring Data Locality

## Problem

Moving large datasets across the network unnecessarily.

---

## Impact

* Network bottlenecks
* Slow processing

---

## Solution

Process data where it resides.

This is one of Hadoop's biggest advantages.

---

# Mistake 6: Not Monitoring Cluster Health

## Problem

Never checking cluster status.

---

## Impact

* Dead DataNodes remain unnoticed
* Under-replicated blocks accumulate

---

## Useful Commands

```bash
hdfs dfsadmin -report
```

```bash
jps
```

---

# Mistake 7: Ignoring Safe Mode

## Problem

Trying to upload files while NameNode is in Safe Mode.

---

## Error

```text
Cannot create file.
NameNode is in Safe Mode.
```

---

## Solution

Check status:

```bash
hdfs dfsadmin -safemode get
```

Leave Safe Mode:

```bash
hdfs dfsadmin -safemode leave
```

---

# Mistake 8: Confusing Local Path and HDFS Path

## Wrong

```bash
hdfs dfs -cat /home/cloudera/data.csv
```

---

## Why Wrong?

This is a local path.

HDFS commands require HDFS paths.

---

## Correct

```bash
hdfs dfs -cat /data/data.csv
```

---

# Mistake 9: Not Using Compression

## Problem

Storing massive datasets uncompressed.

---

## Impact

* More storage usage
* Increased network traffic

---

## Solution

Use:

* Snappy
* Gzip
* Bzip2

---

# Mistake 10: Poor Block Size Selection

## Problem

Using inappropriate block sizes.

---

## Impact

Too Small:

* More metadata
* More NameNode overhead

Too Large:

* Reduced parallelism

---

## Best Practice

Use default:

```text
128 MB
```

unless a specific use case requires otherwise.

---

# Mistake 11: Ignoring Permissions

## Problem

Incorrect HDFS permissions.

---

## Example

```bash
hdfs dfs -chmod 000 /data
```

---

## Impact

Users cannot access data.

---

## Solution

Apply permissions carefully.

---

# Mistake 12: Not Checking Failed Jobs

## Problem

Ignoring failed MapReduce jobs.

---

## Impact

Root causes remain unresolved.

---

## Best Practice

Check:

* Logs
* ResourceManager UI
* NodeManager logs

---

# Real-World Scenario

## Scenario

A company stores 20 million log files.

Average file size:

```text
5 KB
```

---

## Result

NameNode memory becomes exhausted.

Cluster performance degrades.

---

## Solution

Merge files into:

* Parquet
* ORC

Reduce metadata overhead.

---

# Interview Questions

## Beginner

1. What is the Small Files Problem?
2. Why is replication important?

## Intermediate

3. What happens if replication factor is 1?
4. Why is monitoring important?

## Advanced

5. How does the Small Files Problem affect NameNode?
6. How would you optimize HDFS storage?
7. How does data locality improve performance?

---

# Best Practices Checklist

✅ Use large files

✅ Monitor cluster health regularly

✅ Use proper replication factor

✅ Use compression

✅ Organize directories properly

✅ Monitor DataNodes

✅ Follow data locality principles

✅ Use suitable block sizes

---

# Key Takeaways

* Small files are one of the biggest HDFS problems.
* Replication should be configured carefully.
* Cluster monitoring is essential.
* Compression improves storage efficiency.
* Data locality improves processing speed.
* Proper directory structure simplifies management and troubleshooting.
