---

id: glossary
title: Glossary
sidebar_position: 1
-------------------

# Glossary

> MongoDB core terminology — interview‑friendly, one‑line definitions (logical order)

---

1. **MongoDB** — Document‑oriented NoSQL database designed for scalability and flexibility.
2. **mongod** — Primary MongoDB server process that stores data and handles requests.
3. **mongos** — Query router for sharded clusters.
4. **mongo / mongosh** — Interactive MongoDB shell client.
5. **Document** — BSON object stored in a collection; atomic unit of data.
6. **Field** — Key‑value pair inside a document.
7. **Embedded Document** — Nested document stored inside another document.
8. **Array** — Ordered list of values within a document.
9. **Collection** — Group of documents within a database.
10. **Capped Collection** — Fixed‑size collection with circular insertion order.
11. **Time‑Series Collection** — Optimized collection for time‑based measurements.
12. **View** — Read‑only aggregation result exposed as a collection.
13. **Database** — Logical container for collections.
14. **Namespace** — Combination of database and collection name.
15. **BSON** — Binary JSON encoding used internally by MongoDB.
16. **ObjectId** — Default 12‑byte unique identifier for `_id`.
17. **_id** — Mandatory primary key field for documents.
18. **Extended JSON** — JSON representation of BSON types.
19. **Index** — Data structure enabling efficient query execution.
20. **Compound Index** — Index on multiple fields.
21. **Multikey Index** — Index created on array fields.
22. **Text Index** — Index supporting full‑text search.
23. **Hashed Index** — Index that hashes field values.
24. **Wildcard Index** — Index covering dynamic or unknown fields.
25. **Partial Index** — Index built on a subset of documents.
26. **Sparse Index** — Index excluding documents missing indexed fields.
27. **TTL Index** — Index that automatically deletes expired documents.
28. **Unique Index** — Index enforcing unique values.
29. **Index Intersection** — Query using multiple indexes simultaneously.
30. **Covered Query** — Query satisfied entirely by an index.
31. **Query Planner** — Component that selects optimal query plans.
32. **Winning Plan** — Chosen execution plan for a query.
33. **Explain Plan** — Detailed execution statistics for a query.
34. **Query Predicate** — Filter condition in a query.
35. **Projection** — Selection of fields returned by a query.
36. **Sort** — Ordering of query results.
37. **Cursor** — Pointer to query result set.
38. **Batch Size** — Number of documents returned per cursor batch.
39. **Aggregation** — Framework for data processing and transformation.
40. **Aggregation Pipeline** — Ordered stages processing documents.
41. **Stage** — Single transformation step in a pipeline.
42. **$match** — Filters documents in aggregation.
43. **$project** — Reshapes documents.
44. **$group** — Groups documents and computes aggregates.
45. **$sort** — Orders documents.
46. **$limit** — Restricts number of documents.
47. **$skip** — Skips documents.
48. **$lookup** — Performs left outer join.
49. **$unwind** — Deconstructs array fields.
50. **$facet** — Runs multiple pipelines in parallel.
51. **Accumulator** — Operator computing aggregated values.
52. **Expression** — Computation evaluated per document.
53. **Pipeline Optimization** — Internal reordering of stages.
54. **Replica Set** — Group of mongod nodes for redundancy.
55. **Primary** — Node receiving writes.
56. **Secondary** — Node replicating data from primary.
57. **Arbiter** — Voting‑only replica set member.
58. **Election** — Process selecting a primary.
59. **Oplog** — Replication log of operations.
60. **Rollback** — Reversion of unreplicated writes.
61. **Write Concern** — Write durability guarantee.
62. **Read Concern** — Read consistency guarantee.
63. **Read Preference** — Node selection strategy for reads.
64. **Majority** — Consensus of replica set members.
65. **Journaling** — Write‑ahead logging for durability.
66. **Journal Commit** — Flush of journal to disk.
67. **Flow Control** — Replication lag throttling mechanism.
68. **Heartbeat** — Replica set health check.
69. **Priority** — Election weight of a replica set member.
70. **Hidden Node** — Non‑readable replica set member.
71. **Delayed Node** — Replica set member with replication delay.
72. **Sharding** — Horizontal data partitioning.
73. **Shard** — Data partition in a sharded cluster.
74. **Shard Key** — Field determining data distribution.
75. **Chunk** — Contiguous shard key range.
76. **Balancer** — Process redistributing chunks.
77. **Config Server** — Stores cluster metadata.
78. **Zone** — Shard key range placement control.
79. **Hashed Sharding** — Sharding using hashed shard keys.
80. **Range Sharding** — Sharding using ordered ranges.
81. **Scatter‑Gather** — Query sent to multiple shards.
82. **Targeted Query** — Query routed to specific shards.
83. **Chunk Migration** — Movement of chunks between shards.
84. **Resharding** — Changing shard key of a collection.
85. **Transaction** — Multi‑document atomic operation.
86. **ACID** — Atomicity, Consistency, Isolation, Durability guarantees.
87. **Session** — Logical context for operations.
88. **Snapshot Isolation** — Consistent transactional view.
89. **Two‑Phase Commit** — Distributed transaction protocol.
90. **Retryable Writes** — Automatically retried idempotent writes.
91. **Change Stream** — Real‑time data change notifications.
92. **Resume Token** — Marker to resume change streams.
93. **Logical Session ID** — Identifier for client sessions.
94. **Schema Validation** — Enforced document structure rules.
95. **JSON Schema** — Validation specification format.
96. **Validation Level** — Strictness of schema enforcement.
97. **Validation Action** — Error or warning behavior.
98. **Collation** — Locale‑specific string comparison rules.
99. **Case Sensitivity** — Collation behavior for casing.
100. **Numeric Ordering** — Collation numeric comparison.
101. **TTL Monitor** — Background expiration process.
102. **Profiler** — Captures database operation metrics.
103. **Slow Query Log** — Logged long‑running operations.
104. **FTDC** — Full‑Time Diagnostic Data Capture.
105. **Server Status** — Runtime metrics snapshot.
106. **CurrentOp** — In‑flight operation inspection.
107. **Lock** — Concurrency control mechanism.
108. **WiredTiger** — Default MongoDB storage engine.
109. **Storage Engine** — Component managing data persistence.
110. **Compression** — Disk space reduction technique.
111. **Checkpoint** — Consistent on‑disk snapshot.
112. **Cache** — In‑memory data storage.
113. **Eviction** — Removal of data from cache.
114. **Page Fault** — Disk read due to missing cache page.
115. **Document Growth** — Increase in document size.
116. **Padding Factor** — Reserved space for updates.
117. **Power of Two Allocation** — Storage allocation strategy.
118. **Index Build** — Creation of index structures.
119. **Foreground Index Build** — Blocking index creation.
120. **Background Index Build** — Non‑blocking index creation.
121. **Rolling Index Build** — Index creation without downtime.
122. **Compact** — Storage space reclamation.
123. **Repair** — Data file reconstruction.
124. **Backup** — Data copy for recovery.
125. **Restore** — Recovery from backup.
126. **Point‑in‑Time Recovery** — Restore to specific timestamp.
127. **Snapshot Backup** — Filesystem‑level backup.
128. **Logical Backup** — Dump‑based backup.
129. **Atlas** — MongoDB managed cloud service.
130. **Atlas Search** — Lucene‑based full‑text search.
131. **Atlas Data Federation** — Query data across sources.
132. **Realm / App Services** — Backend services for MongoDB.
133. **Trigger** — Event‑driven serverless function.
134. **Data API** — HTTPS access to MongoDB.
135. **Role** — Set of privileges.
136. **Privilege** — Permission to perform actions.
137. **User** — Authenticated identity.
138. **Authentication** — Identity verification process.
139. **Authorization** — Access control enforcement.
140. **SCRAM** — Default authentication mechanism.
141. **x.509** — Certificate‑based authentication.
142. **LDAP** — External authentication integration.
143. **Encryption at Rest** — Disk data encryption.
144. **Encryption in Transit** — TLS network encryption.
145. **Audit Log** — Security event record.
146. **Failover** — Automatic primary switch.
147. **High Availability** — Continuous service operation.
148. **Scalability** — Ability to handle growth.
149. **Horizontal Scaling** — Scaling via sharding.
150. **Vertical Scaling** — Scaling via hardware upgrades.
