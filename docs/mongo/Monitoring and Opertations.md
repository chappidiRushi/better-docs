---

id: mongodb-monitoring-operations
title: Monitoring and Operations
sidebar_position: 18
--------------------

# 18. Monitoring and Operations

> Observability, performance tracking, and operational safety for MongoDB clusters

---

## Metrics and Telemetry

> Continuous measurement of server, storage, and query behavior

**Interview points**

* Exposed via `serverStatus`, `dbStats`, `collStats`
* Includes memory, connections, locks, cache, ops/sec
* Foundation for alerting and capacity planning

<details>
<summary>Examples (Node.js)</summary>

```js
// Server-level telemetry
const status = await adminDb.command({ serverStatus: 1 });
status.mem;            // RAM usage
status.connections;    // current / available
status.opcounters;     // insert/query/update/delete
```

</details>

---

## Profiling

> Capturing query execution details at runtime

**Interview points**

* Uses system.profile collection
* Levels: 0 (off), 1 (slow ops), 2 (all ops)
* High overhead at level 2

<details>
<summary>Examples (Node.js)</summary>

```js
// Enable slow operation profiling
db.setProfilingLevel(1, { slowms: 100 });

// Read profiler data
const slowQueries = await db.collection('system.profile').find().limit(5).toArray();
```

</details>

---

## Slow Query Logs

> Logging queries exceeding latency thresholds

**Interview points**

* Controlled via `slowms` and `logLevel`
* Logged to mongod logs
* Preferred over profiler in production

<details>
<summary>Examples (Node.js)</summary>

```js
// Adjust slow query threshold
await adminDb.command({ setParameter: 1, slowms: 50 });
```

</details>

---

## Capacity Planning

> Ensuring resources meet workload demands

**Interview points**

* Based on working set size
* Monitor disk IOPS, cache eviction, replication lag
* Scale reads, writes, or storage independently

<details>
<summary>Examples (Node.js)</summary>

```js
// Collection-level stats
const stats = await db.collection('orders').stats();
stats.storageSize;   // disk
stats.totalIndexSize; // index footprint
```

</details>

---

## Alerting Strategies

> Proactive detection of failures and regressions

**Interview points**

* Alert on lag, memory pressure, disk usage, elections
* Prefer symptom-based alerts over raw metrics
* Avoid noisy thresholds

<details>
<summary>Examples (Node.js)</summary>

```js
// Replication lag check
const repl = await adminDb.command({ replSetGetStatus: 1 });
repl.members.forEach(m => {
  if (m.stateStr === 'SECONDARY') {
    console.log(m.name, m.optimeDate);
  }
});
```

</details>

---

## Notes

* Always baseline metrics under normal load
* Monitor trends, not snapshots

## Caveats

* Profiling can distort performance
* Excessive alerts cause alert fatigue
* Metrics without context are misleading
