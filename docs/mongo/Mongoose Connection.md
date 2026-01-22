---

id: mongoose-connections-topology
title: Mongoose Connections and Topology
sidebar_position: 24
--------------------

# 24. Mongoose Connections and Topology

> How Mongoose manages connections, pools, and MongoDB cluster topology

---

## Connection Lifecycle

> Establishment, readiness, error handling, and teardown of connections

**Interview points**

* Built on top of MongoDB Node.js driver
* Connection state machine (connecting → connected → disconnected)
* Emits lifecycle events

<details>
<summary>Examples (Node.js)</summary>

```js
import mongoose from 'mongoose';

mongoose.connection.on('connected', () => console.log('connected'));
mongoose.connection.on('error', err => console.error(err));
mongoose.connection.on('disconnected', () => console.log('disconnected'));

await mongoose.connect('mongodb://localhost:27017/app');
```

</details>

---

## Connection Pooling

> Reuse of TCP connections for concurrent operations

**Interview points**

* Pool per server
* Controlled via driver options
* Prevents connection thrashing

<details>
<summary>Examples (Node.js)</summary>

```js
await mongoose.connect(uri, {
  maxPoolSize: 20,        // number
  minPoolSize: 5,         // number
  maxIdleTimeMS: 30000,   // number
  waitQueueTimeoutMS: 5000, // number
});
```

</details>

---

## Multiple Connections

> Managing separate connections for isolation or multi-tenancy

**Interview points**

* Each connection has its own models
* Avoids cross-database contamination
* Useful for sharding by tenant

<details>
<summary>Examples (Node.js)</summary>

```js
const connA = mongoose.createConnection('mongodb://localhost:27017/dbA');
const connB = mongoose.createConnection('mongodb://localhost:27017/dbB');

const UserA = connA.model('User', userSchema);
const UserB = connB.model('User', userSchema);
```

</details>

---

## Replica Sets and Shards

> Awareness of MongoDB cluster topology

**Interview points**

* Topology handled by driver SDAM
* Automatic primary discovery
* Mongoose is topology-agnostic

<details>
<summary>Examples (Node.js)</summary>

```js
await mongoose.connect('mongodb://n1,n2,n3/app?replicaSet=rs0');

// Sharded cluster via mongos
await mongoose.connect('mongodb://mongos1,mongos2/app');
```

</details>

---

## Read and Write Concerns

> Consistency and durability controls at connection level

**Interview points**

* Defaults inherited from driver
* Can be overridden per query
* Critical in multi-region setups

<details>
<summary>Examples (Node.js)</summary>

```js
await mongoose.connect(uri, {
  readConcern: { level: 'majority' }, // local | majority | snapshot
  writeConcern: { w: 'majority', j: true }, // number | 'majority'
});
```

</details>

---

## Graceful Shutdown

> Cleanly closing connections during process termination

**Interview points**

* Prevents in-flight operation loss
* Important for containers and serverless
* Must await disconnect

<details>
<summary>Examples (Node.js)</summary>

```js
process.on('SIGINT', async () => {
  await mongoose.disconnect();
  process.exit(0);
});
```

</details>

---

## Notes

* One process should reuse a single primary connection
* Pool sizing directly affects latency

## Caveats

* Over-pooling wastes memory
* Multiple connections complicate debugging
* Improper shutdown causes connection leaks
