---

id: mongodb-limits-edge-cases
title: MongoDB Limits, Edge Cases, and Anti-Patterns
sidebar_position: 21
--------------------

# MongoDB Limits, Edge Cases, and Anti-Patterns

> Hard limits, pathological patterns, and common mistakes that break MongoDB at scale

---

## Document and Index Limits

> Maximum sizes and structural constraints enforced by the server

**Interview points**

* Max document size: **16MB**
* Index key limit: **1024 bytes per field**
* Compound index key count limit

<details>
<summary>Examples (Node.js)</summary>

```js
// Oversized document example
const bigDoc = { data: 'x'.repeat(17 * 1024 * 1024) }; // >16MB
await db.collection('test').insertOne(bigDoc); // fails
```

</details>

---

## Transaction Limits

> Constraints on multi-document transactions

**Interview points**

* Max transaction size: 16MB oplog entry
* Max lifetime: 60 seconds (default)
* No cross-shard writes without sharding awareness

<details>
<summary>Examples (Node.js)</summary>

```js
const session = client.startSession();
session.startTransaction({
  readConcern: { level: 'snapshot' }, // local | majority | snapshot
  writeConcern: { w: 'majority' },     // number | 'majority'
});

// Too many ops can abort transaction
```

</details>

---

## Aggregation Limits

> Resource and execution constraints in pipelines

**Interview points**

* 100MB memory limit per stage (unless allowDiskUse)
* 50 stages max
* Blocking stages increase latency

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').aggregate([
  { $group: { _id: '$userId', total: { $sum: '$amount' } } }
], {
  allowDiskUse: true, // boolean
});
```

</details>

---

## Hot Documents

> Excessive contention on a single document

**Interview points**

* Single-document write lock contention
* Common with counters and queues
* Causes latency spikes

<details>
<summary>Examples (Node.js)</summary>

```js
// Anti-pattern: single counter doc
await db.collection('stats').updateOne(
  { _id: 'global' },
  { $inc: { count: 1 } }
);
```

</details>

---

## Massive Arrays

> Unbounded array growth inside documents

**Interview points**

* Document growth causes relocations
* Querying large arrays is expensive
* Indexing arrays increases index size

<details>
<summary>Examples (Node.js)</summary>

```js
// Anti-pattern: ever-growing array
await db.collection('users').updateOne(
  { _id: userId },
  { $push: { events: event } }
);
```

</details>

---

## Unindexed Queries

> Collection scans due to missing or unusable indexes

**Interview points**

* Cause high CPU and IO
* Block cache with useless reads
* Easily detectable via explain

<details>
<summary>Examples (Node.js)</summary>

```js
// Collection scan
await db.collection('orders').find({ status: 'PENDING' }).toArray();

// Detect
await db.collection('orders')
  .find({ status: 'PENDING' })
  .explain('executionStats');
```

</details>

---

## Notes

* Most MongoDB outages are self-inflicted
* Limits exist to protect cluster stability

## Caveats

* Ignoring limits leads to cascading failures
* Anti-patterns often appear "fine" at small scale
* Always validate with explain and load tests
