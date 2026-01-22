---
id: mongodb-performance-optimization
title: Performance and Optimization
sidebar_position: 13
--------------------

# Performance and Optimization

> Designing MongoDB systems for predictable latency and high throughput

---

## Working Set Model

> Performance depends on how much active data fits in memory

**Interview points**

* Working set = frequently accessed data + indexes
* Should fit in WiredTiger cache
* Cache misses cause disk IO

<details>
<summary>Examples (Node.js)</summary>

```js
// Inspect cache usage and eviction
await adminDb.command({ serverStatus: 1 })
  .then(r => r.wiredTiger.cache);
```

</details>

---

## RAM vs Disk Access

> Memory access is orders of magnitude faster than disk

**Interview points**

* RAM hits = microseconds
* Disk hits = milliseconds
* SSD strongly recommended

<details>
<summary>Examples (Node.js)</summary>

```js
// Force cold read (anti-pattern)
await db.collection('logs')
  .find({ level: 'error' })
  .hint({ _id: 1 })
  .toArray();
```

</details>

---

## Query Shape Stability

> Consistent query patterns improve plan caching

**Interview points**

* Same query shape → cached plan reuse
* Dynamic fields break cache
* Avoid unbounded predicates

<details>
<summary>Examples (Node.js)</summary>

```js
// Stable query shape
await db.collection('users').find({ status: 'active' });

// Unstable query shape (anti-pattern)
await db.collection('users').find({ [dynamicField]: value });
```

</details>

---

## Index Selectivity

> Highly selective indexes reduce scanned documents

**Interview points**

* High cardinality preferred
* Equality before range
* Compound index order matters

<details>
<summary>Examples (Node.js)</summary>

```js
// High selectivity index
await db.collection('users').createIndex({ email: 1 });

// Low selectivity index (anti-pattern)
await db.collection('users').createIndex({ isActive: 1 });
```

</details>

---

## Write Optimization

> Optimizing throughput and durability tradeoffs

**Interview points**

* Batch writes
* Minimize index count
* Use unordered bulk ops

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('events').insertMany(docs, {
  ordered: false // boolean
});
```

</details>

---

## Read Optimization

> Reducing latency and IO for read-heavy workloads

**Interview points**

* Covered queries
* Projection reduction
* Read from secondaries if allowed

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').find(
  { email: 'a@x.com' },
  { projection: { _id: 0, email: 1 } }
).hint({ email: 1 });
```

</details>

---

## Benchmarking Strategies

> Measuring real-world performance correctly

**Interview points**

* Test production-like data
* Warm cache before measuring
* Measure p95 / p99 latency

<details>
<summary>Examples (Node.js)</summary>

```js
console.time('query');
await db.collection('orders').find({ status: 'paid' }).toArray();
console.timeEnd('query');
```

</details>

---

## Notes

* Performance is workload-dependent
* Schema and index design dominate outcomes

## Caveats

* Over-indexing hurts writes
* Cache misses dominate latency
* Benchmarks without warmup are misleading
