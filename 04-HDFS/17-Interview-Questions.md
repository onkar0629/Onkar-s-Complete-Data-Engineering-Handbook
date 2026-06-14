HDFS Interview Questions (Master Guide)
Introduction
This document contains the most frequently asked HDFS interview questions from:
Data Engineer Interviews
Hadoop Developer Interviews
Big Data Engineer Interviews
Hadoop Administrator Interviews
Beginner Level Questions
1. What is HDFS?
Answer:
HDFS (Hadoop Distributed File System) is a distributed storage system designed to store and process large datasets across multiple machines.
2. Why was HDFS created?
Answer:
Traditional file systems cannot efficiently store and process TBs/PBs of data.
HDFS provides:
Scalability
Fault Tolerance
High Throughput
3. What are the main components of HDFS?
Answer:
NameNode
DataNode
4. What is a NameNode?
Answer:
The master node that stores metadata about files and blocks.
5. What is a DataNode?
Answer:
Worker node that stores actual data blocks.
6. Does NameNode store actual data?
Answer:
No.
It stores only metadata.
7. What does metadata contain?
Answer:
File names
Directory structure
Permissions
Block locations
Replication information
8. What is a block?
Answer:
The smallest storage unit in HDFS.
9. What is the default block size?
Answer:
128 MB
10. Why does HDFS use large blocks?
Answer:
To reduce:
Metadata overhead
Disk seek operations
Intermediate Questions
11. What is replication?
Answer:
Creating multiple copies of a block across different DataNodes.
12. What is the default replication factor?
Answer:
3
13. Why is replication important?
Answer:
Provides:
Fault Tolerance
High Availability
14. What happens if a DataNode fails?
Answer:
NameNode detects failure through missing heartbeats and starts re-replication.
15. What is a heartbeat?
Answer:
A signal sent by DataNode to NameNode indicating it is alive.
16. Heartbeat interval?
Answer:
Approximately:
3 seconds
17. What is a block report?
Answer:
A report sent by DataNodes containing block information.
18. What is Rack Awareness?
Answer:
Replica placement strategy that distributes replicas across multiple racks.
19. Why is Rack Awareness needed?
Answer:
Protects against rack-level failures.
20. What is Data Locality?
Answer:
Moving computation to data instead of moving data to computation.
HDFS Read/Write Questions
21. Explain HDFS Read Operation.
Answer:
Client contacts NameNode
NameNode returns block locations
Client contacts DataNodes
Data is read
22. Does NameNode participate in data transfer?
Answer:
No.
Only metadata transfer.
23. Explain HDFS Write Operation.
Answer:
Client contacts NameNode
NameNode selects DataNodes
Pipeline created
Data written
Replication completed
24. What is a replication pipeline?
Answer:
Chain of DataNodes used during write operations.
Example:
Client → DN1 → DN2 → DN3
25. Why are acknowledgements required?
Answer:
To ensure successful block replication.
NameNode Questions
26. What files store NameNode metadata?
Answer:
FsImage
Edit Logs
27. What is FsImage?
Answer:
Snapshot of filesystem metadata.
28. What are Edit Logs?
Answer:
Records of filesystem changes.
29. What happens during NameNode startup?
Answer:
Load FsImage
Load Edit Logs
Apply edits
Start service
30. Why is NameNode memory important?
Answer:
Metadata is stored in RAM.
Secondary NameNode Questions
31. What is Secondary NameNode?
Answer:
Checkpointing service for NameNode metadata.
32. Is Secondary NameNode a backup NameNode?
Answer:
❌ No
33. What is checkpointing?
Answer:
Merging FsImage and Edit Logs.
34. Why is checkpointing required?
Answer:
Reduces NameNode restart time.
High Availability Questions
35. Why do we need High Availability?
Answer:
To remove NameNode Single Point of Failure.
36. Components of HA?
Answer:
Active NameNode
Standby NameNode
ZooKeeper
JournalNodes
37. What is failover?
Answer:
Standby NameNode becoming Active.
38. What is ZooKeeper's role?
Answer:
Coordinates failover.
39. What is Split Brain?
Answer:
Both NameNodes become Active simultaneously.
40. How is Split Brain prevented?
Answer:
ZooKeeper coordination.
Federation Questions
41. What is HDFS Federation?
Answer:
Multiple NameNodes managing different namespaces.
42. Why was Federation introduced?
Answer:
To solve metadata scalability issues.
43. What is a namespace?
Answer:
Filesystem hierarchy managed by a NameNode.
44. What is a block pool?
Answer:
Collection of blocks managed by a NameNode.
45. Can a DataNode serve multiple NameNodes?
Answer:
Yes.
Production Scenario Questions
46. What is the Small Files Problem?
Answer:
Millions of small files create metadata overload.
47. Why are small files dangerous?
Answer:
Metadata consumes NameNode memory.
48. How can you solve the Small Files Problem?
Answer:
Use:
Parquet
ORC
Avro
49. What happens when a rack fails?
Answer:
Replicas on other racks remain available.
50. What happens if all replicas are lost?
Answer:
Data loss occurs.
Frequently Asked Command Questions
51. List HDFS files
hdfs dfs -ls
52. Upload file
hdfs dfs -put
53. Download file
hdfs dfs -get
54. Check cluster health
hdfs dfsadmin -report
55. Check filesystem health
hdfs fsck /
56. Change replication
hdfs dfs -setrep
57. Safe mode status
hdfs dfsadmin -safemode get
58. Check Hadoop processes
jps
Advanced Scenario Questions
59. A DataNode fails. What happens?
Answer:
Heartbeat stops
NameNode marks dead
Re-replication starts
60. NameNode crashes in non-HA cluster.
Answer:
Cluster becomes unavailable.
61. NameNode crashes in HA cluster.
Answer:
Standby NameNode takes over.
62. Why is replication factor usually 3?
Answer:
Balances fault tolerance and storage cost.
63. Difference between HA and Federation?
HA	Federation
Fault Tolerance	Scalability
Active + Standby	Multiple NameNodes
Solves SPOF	Solves Metadata Bottleneck
64. Why does Hadoop prefer Node Local execution?
Answer:
Lowest network cost.
65. Difference between Node Local and Rack Local?
Answer:
Node Local = Same machine
Rack Local = Same rack
66. What happens when disk becomes full?
Answer:
Write operations fail.
67. How do you balance a cluster?
hdfs balancer
68. What causes under-replicated blocks?
Answer:
DataNode failure
Disk failure
Network issues
69. What causes corrupt blocks?
Answer:
Disk corruption
Hardware failures
70. Why is HDFS write-once-read-many?
Answer:
Optimized for large-scale analytics workloads.
Top 20 One-Line Revision Questions
NameNode?
Stores metadata.
DataNode?
Stores actual data.
Default Block Size?
128 MB
Default Replication?
3
Heartbeat?
Alive signal from DataNode.
Block Report?
Block information report.
FsImage?
Metadata snapshot.
Edit Logs?
Metadata changes.
Secondary NameNode?
Checkpointing service.
HA?
Removes SPOF.
ZooKeeper?
Failover coordinator.
Federation?
Multiple NameNodes.
Block Pool?
Blocks managed by a NameNode.
Data Locality?
Move computation to data.
Rack Awareness?
Cross-rack replica placement.
Safe Mode?
Read-only NameNode state.
Small Files Problem?
Metadata overload.
Re-Replication?
Replica recovery process.
FSCK?
Filesystem health check.
Balancer?
Redistributes blocks evenly.
Most Common Interview Trap Questions
Q: Does NameNode store data?
❌ No
Q: Is Secondary NameNode a backup?
❌ No
Q: Does replication alone protect against rack failure?
❌ No
Rack Awareness is also required.
Q: Can HDFS run without DataNodes?
❌ No
Q: Can HDFS run without NameNode?
❌ No
Unless HA failover occurs.
Final HDFS Interview Formula
Remember these 10 concepts:
NameNode

DataNode

Blocks

Replication

Heartbeats

Block Reports

Rack Awareness

Data Locality

High Availability

Federation
