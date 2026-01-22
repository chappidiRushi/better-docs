---

id: mongodb-wiredtiger
title: Storage Engine (WiredTiger)
sidebar_position: 12
--------------------

# 12. Storage Engine (WiredTiger)

> Default MongoDB storage engine providing high concurrency, compression, and durability

---

## WiredTiger Architecture

> Log-structured storage engine with B-Tree data organization

**Interview points**

* Collection and index data stored separately
* Uses B-Trees with page-level locking
* Pluggable storage engine

<details>
<summary>Examples (Node.js)</summary>

```js
// WiredTiger is enabled by default
// Verify engine
await adminDb.command({ serverStatus: 1 }).then(r => r.storageEngine);
```

</details>

---

## MVCC

> Multi-Version Concurrency Control for isolation

**Interview points**

* Readers don’t block writers
* Snapshot isolation
* Versioned documents

<details>
<summary>Examples (Node.js)</summary>

```js
// Snapshot isolation via transaction
const session = client.startSession();

session.startTransaction({
  readConcern: { level: 'snapshot' }
});

await db.collection('orders').findOne({ _id: 1 }, { session });

await session.commitTransaction();
```

</details>

---

## Journaling

> Write-ahead logging for durability

**Interview points**

* Sequential journal writes
* Crash recovery support
* Enabled by default

<details>
<summary>Examples (Node.js)</summary>

```js
// Check journaling status
await adminDb.command({ getCmdLineOpts: 1 });
```

</details>

---

## Compression

> Reduces disk and cache usage

**Interview points**

* Block compression (Snappy / Zstd / Zlib)
* Collection-level configuration
* Index compression supported

<details>
<summary>Examples (Node.js)</summary>

```js
// Create collection with compression
await db.createCollection('logs', {
  storageEngine: {
    wiredTiger: {
      blockCompressor: 'zstd' // snappy | zlib | zstd | none
    }
  }
});
```

</details>

---

## Cache Management

> In-memory cache for hot data

**Interview points**

* Default ~50% RAM
* Shared between indexes and data
* LRU eviction

<details>
<summary>Examples (Node.js)</summary>

```js
// Inspect cache usage
await adminDb.command({ serverStatus: 1 }).then(r => r.wiredTiger.cache);
```

</details>

---

## Read vs Write Amplification

> Tradeoff between read efficiency and write cost

**Interview points**

* Write amplification due to journaling + compaction
* Read amplification reduced by caching
* Compression increases CPU cost

<details>
<summary>Examples (Node.js)</summary>

```js
// High write workload example
await db.collection('metrics').updateOne(
  { _id: 1 },
  { $inc: { count: 1 } },
  { upsert: true }
);
```

</details>

---

## Notes

* WiredTiger is optimized for SSDs
* Enables high concurrency workloads

## Caveats

* Heavy writes increase write amplification
* Cache pressure impacts latency
* Compression trades CPU for IO savings
