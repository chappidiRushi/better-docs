---

id: mongodb-deployment-architectures
title: Deployment Architectures
sidebar_position: 19
--------------------

# 19. Deployment Architectures

> Common MongoDB deployment topologies and their operational tradeoffs

---

## Single Node

> Standalone `mongod` without replication or sharding

**Interview points**

* No high availability
* No automatic failover
* Used for dev, testing, or edge workloads

<details>
<summary>Examples (Node.js)</summary>

```js
// Connect to standalone MongoDB
import { MongoClient } from 'mongodb';

const client = new MongoClient('mongodb://localhost:27017');
await client.connect();
const db = client.db('app');
```

</details>

---

## Replica Sets

> Group of mongod instances with primary-secondary replication

**Interview points**

* Provides high availability
* Automatic failover via elections
* Majority write concern ensures durability

<details>
<summary>Examples (Node.js)</summary>

```js
// Replica set connection string
const uri = 'mongodb://n1,n2,n3/app?replicaSet=rs0';
const client = new MongoClient(uri);
await client.connect();
```

</details>

---

## Sharded Clusters

> Horizontally scaled cluster using shard key partitioning

**Interview points**

* Requires mongos, config servers, shards
* Scales writes and storage
* Shard key choice is critical

<details>
<summary>Examples (Node.js)</summary>

```js
// Connect via mongos
const client = new MongoClient('mongodb://mongos1,mongos2/app');
await client.connect();
```

</details>

---

## Multi-Region Deployments

> Distributing replica set members or shards across regions

**Interview points**

* Improves read latency
* Increases write latency
* Requires careful write concern and read preference

<details>
<summary>Examples (Node.js)</summary>

```js
// Prefer nearest node
const client = new MongoClient(uri, {
  readPreference: 'nearest', // primary | secondary | nearest
});
```

</details>

---

## Kubernetes Deployments

> Running MongoDB on container orchestration platforms

**Interview points**

* StatefulSets required
* PersistentVolumes mandatory
* Automation via Operators

<details>
<summary>Examples (Node.js)</summary>

```js
// App-side connection remains unchanged
const client = new MongoClient('mongodb://mongo-0.mongo-svc:27017');
await client.connect();
```

</details>

---

## Notes

* Architecture choice impacts consistency, latency, and ops complexity
* Prefer replica sets over standalone even in small prod setups

## Caveats

* Sharding adds operational overhead
* Multi-region writes are expensive
* Kubernetes requires deep ops expertise
