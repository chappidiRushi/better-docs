---
id: mongodb-sharding
title: Sharding
sidebar_position: 11
--------------------

# 11. Sharding

> Horizontal scaling by distributing data across multiple shards

---

## Sharding Architecture

> Cluster composed of shards, config servers, and mongos routers

**Interview points**

* Shards store data (replica sets)
* Config servers store metadata
* mongos routes queries

<details>
<summary>Examples (Node.js)</summary>

```js
// Connect via mongos
const client = new MongoClient('mongodb://mongos1,mongos2');
await client.connect();
```

</details>

---

## Shard Keys

> Field(s) determining data distribution

**Interview points**

* Immutable once chosen
* High cardinality preferred
* Determines query routing

<details>
<summary>Examples (Node.js)</summary>

```js
// Enable sharding and shard a collection
await adminDb.command({ enableSharding: 'shop' });

await adminDb.command({
  shardCollection: 'shop.orders',
  key: { userId: 1 } // ranged | hashed
});
```

</details>

---

## Chunk Management

> Data is partitioned into chunks per shard key range

**Interview points**

* Default chunk size ~128MB
* Chunks split automatically
* Moved between shards

<details>
<summary>Examples (Node.js)</summary>

```js
// View chunk metadata
await adminDb.command({ listChunks: 'shop.orders' });
```

</details>

---

## Balancer Internals

> Background process equalizing chunk distribution

**Interview points**

* Runs on config servers
* Migrates chunks
* Can be paused

<details>
<summary>Examples (Node.js)</summary>

```js
// Disable balancer
await adminDb.command({ balancerStop: 1 });

// Enable balancer
await adminDb.command({ balancerStart: 1 });
```

</details>

---

## Hotspot Avoidance

> Preventing uneven load on specific shards

**Interview points**

* Avoid monotonically increasing shard keys
* Use hashed keys or compound keys

<details>
<summary>Examples (Node.js)</summary>

```js
// Hashed shard key to avoid hotspots
await adminDb.command({
  shardCollection: 'logs.events',
  key: { eventId: 'hashed' }
});
```

</details>

---

## Resharding

> Changing shard key without downtime

**Interview points**

* Introduced in MongoDB 5.0+
* Online operation
* Resource intensive

<details>
<summary>Examples (Node.js)</summary>

```js
await adminDb.command({
  reshardCollection: 'shop.orders',
  key: { region: 1, createdAt: 1 }
});
```

</details>

---

## Sharding Limitations

> Constraints and tradeoffs in sharded clusters

**Interview points**

* Scatter-gather queries are expensive
* Unique indexes must include shard key
* Transactions cost more

<details>
<summary>Examples (Node.js)</summary>

```js
// Inefficient scatter-gather query
await db.collection('orders').find({ status: 'pending' }).toArray();
```

</details>

---

## Notes

* Proper shard key is critical
* Sharding scales writes and storage

## Caveats

* Poor shard key causes hotspots
* Resharding impacts cluster resources
* Operational complexity is high
