---

id: mongodb-documents-schema
title: Documents, Collections, and Schema Design
sidebar_position: 4
-------------------

# 4. Documents, Collections, and Schema Design

> Logical data modeling, schema patterns, and growth characteristics

---

## Documents and Collections

> BSON documents grouped into collections; no enforced schema

**Properties**

* Document = atomic unit
* Collection = namespace + indexes
* Implicit collection creation

<details>
<summary>Examples (Node.js)</summary>

```js
import { MongoClient } from 'mongodb';

const client = new MongoClient(uri);
await client.connect();
const db = client.db('app');

// Document insert
await db.collection('users').insertOne({ _id: 1, name: 'A', age: 30 });

// Collection auto-created
await db.collection('orders').insertOne({ orderId: 'O1', total: 100 });

// Explicit collection options
await db.createCollection('logs', {
  capped: false,                  // boolean
  validator: { $jsonSchema: { bsonType: 'object' } }, // object
  validationLevel: 'strict',      // 'strict' | 'moderate'
  validationAction: 'error'       // 'error' | 'warn'
});
```

</details>

---

## Dynamic Schema Implications

> Schema-on-read with optional enforcement

**Implications**

* Application-level validation required
* Schema drift over time
* Mixed-shape documents affect indexes

<details>
<summary>Examples (Node.js)</summary>

```js
// Same collection, different shapes
await db.collection('users').insertMany([
  { _id: 1, name: 'A' },
  { _id: 2, name: 'B', roles: ['admin'] },
  { _id: 3, email: 'c@x.com' }
]);

// Add schema validation later
await db.command({
  collMod: 'users',
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['name'],
      properties: {
        name: { bsonType: 'string' },
        roles: { bsonType: 'array' }
      }
    }
  }
});
```

</details>

---

## Embedding vs Referencing

> Read-path optimization vs write-time flexibility

**Embedding**

* Single-document atomicity
* Faster reads
* Risk of document growth

**Referencing**

* Independent lifecycle
* Requires joins / extra queries

<details>
<summary>Examples (Node.js)</summary>

```js
// Embedding
await db.collection('orders').insertOne({
  _id: 1,
  user: { id: 10, name: 'Alex' },
  items: [{ sku: 'P1', qty: 2 }]
});

// Referencing
await db.collection('orders').insertOne({ _id: 2, userId: 10 });

// Join via aggregation lookup
await db.collection('orders').aggregate([
  {
    $lookup: {
      from: 'users',
      localField: 'userId',
      foreignField: '_id',
      as: 'user'
    }
  }
]).toArray();
```

</details>

---

## Relationship Modeling Patterns

> One-to-one, one-to-many, many-to-many

**Common patterns**

* One-to-few → embed
* One-to-many → bucket / subset
* Many-to-many → referencing + lookup

<details>
<summary>Examples (Node.js)</summary>

```js
// One-to-few
await db.collection('posts').insertOne({
  _id: 1,
  comments: [{ t: 'ok' }, { t: 'nice' }]
});

// One-to-many (bucket pattern)
await db.collection('events').insertOne({
  _id: 'user:1:2025-01',
  events: [] // bounded
});

// Many-to-many
await db.collection('memberships').insertOne({ userId: 1, groupId: 10 });
```

</details>

---

## Schema Evolution

> Forward-compatible changes preferred

**Safe changes**

* Add fields
* Make fields optional
* Rename via dual-write

**Dangerous changes**

* Type changes
* Removing required fields

<details>
<summary>Examples (Node.js)</summary>

```js
// Backward-compatible add
await db.collection('users').updateMany(
  {},
  { $set: { status: 'active' } }
);

// Dual-write rename strategy
await db.collection('users').updateOne(
  { _id: 1 },
  { $set: { givenName: 'A' } }
);
```

</details>

---

## Document Growth and Padding

> In-place growth may cause document relocation

**Facts**

* WiredTiger uses document-level allocation
* Growing documents can fragment storage
* Unbounded arrays are primary risk

<details>
<summary>Examples (Node.js)</summary>

```js
// Risky unbounded growth
await db.collection('logs').updateOne(
  { _id: 1 },
  { $push: { entries: { ts: Date.now() } } }
);

// Safer bucketed pattern
await db.collection('logs').insertOne({
  _id: 'log:1:001',
  entries: [] // capped at app level
});
```

</details>

---

## Namespace Management

> Database + collection naming and limits

**Rules**

* Namespace = `<db>.<collection>`
* Max length: 255 bytes
* System collections are reserved

<details>
<summary>Examples (Node.js)</summary>

```js
// List collections
await db.listCollections().toArray();

// Drop collection
await db.collection('temp').drop();

// Rename collection
await db.admin().command({
  renameCollection: 'app.old',
  to: 'app.new',
  dropTarget: false // boolean
});
```

</details>

---

## Notes

* Schema design is **query-pattern driven**
* Reads are optimized over writes by default
* Most performance issues originate here

## Caveats

* Over-embedding causes document bloat
* Schema drift breaks assumptions
* Poor namespace hygiene complicates ops
