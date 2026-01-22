---

id: mongodb-replication
title: Replication
sidebar_position: 10
--------------------

# Replication

> High availability, redundancy, and consistency via replica sets

---

## Replica Set Architecture

> A group of mongod instances maintaining the same dataset

**Interview points**

* One primary, multiple secondaries
* Automatic failover
* Majority-based consensus

<details>
<summary>Examples (Node.js)</summary>

```js
// Connect to replica set
const client = new MongoClient(
  'mongodb://host1,host2,host3/?replicaSet=rs0'
);

await client.connect();
```

</details>

---

## Oplog Internals

> Capped collection recording all write operations

**Interview points**

* Ordered, idempotent operations
* Secondaries replicate from oplog
* Size impacts rollback window

<details>
<summary>Examples (Node.js)</summary>

```js
// Read oplog (admin only)
const oplog = client.db('local').collection('oplog.rs');

await oplog.find().limit(1).toArray();
```

</details>

---

## Write Concerns

> Level of acknowledgment for write operations

**Interview points**

* Controls durability
* Majority ensures replication
* Tradeoff with latency

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').insertOne(
  { name: 'A' },
  {
    writeConcern: {
      w: 'majority', // number | 'majority'
      j: true,       // boolean
      wtimeout: 5000 // ms
    }
  }
);
```

</details>

---

## Read Concerns

> Guarantees about data visibility

**Interview points**

* Controls consistency
* Snapshot isolation for transactions

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').findOne(
  { _id: 1 },
  {
    readConcern: {
      level: 'majority' // local | available | majority | snapshot
    }
  }
);
```

</details>

---

## Read Preferences

> Determines which replica set member handles reads

**Interview points**

* Primary preferred for consistency
* Secondaries for scaling

<details>
<summary>Examples (Node.js)</summary>

```js
const client = new MongoClient(uri, {
  readPreference: 'secondaryPreferred' // primary | secondary | nearest
});

await client.connect();
```

</details>

---

## Failover and Elections

> Automatic primary selection on failure

**Interview points**

* Raft-like election
* Majority vote required
* Brief write unavailability

<details>
<summary>Examples (Node.js)</summary>

```js
// Step down primary (admin)
await adminDb.command({ replSetStepDown: 60 });
```

</details>

---

## Replication Lag and Rollbacks

> Delay and divergence between primary and secondaries

**Interview points**

* Lag impacts read freshness
* Rollbacks occur on divergent writes

<details>
<summary>Examples (Node.js)</summary>

```js
// Check replication status
await adminDb.command({ replSetGetStatus: 1 });
```

</details>

---

## Notes

* Majority write + read concern ensures strong consistency
* Replication underpins transactions

## Caveats

* Network partitions cause elections
* Long lag increases rollback risk
* Writes briefly unavailable during failover
