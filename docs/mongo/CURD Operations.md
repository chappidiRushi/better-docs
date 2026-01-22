---

id: mongodb-crud
title: CRUD Operations
sidebar_position: 5
-------------------

# CRUD Operations

> Core data manipulation semantics, guarantees, and edge cases

---

## Insert Semantics

> Inserts are atomic per document; no implicit upserts

**Facts**

* Single-document atomicity
* Ordered vs unordered inserts
* `_id` required (auto-generated if missing)

<details>
<summary>Examples (Node.js)</summary>

```js
// insertOne
await db.collection('users').insertOne(
  { _id: 1, name: 'A' },
  { writeConcern: { w: 'majority', j: true } } // writeConcern
);

// insertMany
await db.collection('users').insertMany(
  [{ _id: 2 }, { _id: 3 }],
  { ordered: false } // boolean
);
```

</details>

---

## Query Filters

> Predicate expressions evaluated via query planner

**Capabilities**

* Field operators
* Logical operators
* Array and element matching

<details>
<summary>Examples (Node.js)</summary>

```js
// Simple filter
await db.collection('users').find({ age: { $gte: 18 } }).toArray();

// Logical operators
await db.collection('users').find({
  $and: [
    { status: 'active' },
    { age: { $lt: 50 } }
  ]
}).toArray();

// Array match
await db.collection('users').find({ roles: { $in: ['admin', 'mod'] } }).toArray();
```

</details>

---

## Projection

> Shape control for returned documents

**Rules**

* Inclusion or exclusion (not both, except `_id`)
* Reduces network and decode cost

<details>
<summary>Examples (Node.js)</summary>

```js
// Inclusion projection
await db.collection('users')
  .find({}, { projection: { name: 1, email: 1 } })
  .toArray();

// Exclusion projection
await db.collection('users')
  .find({}, { projection: { password: 0 } })
  .toArray();
```

</details>

---

## Sorting and Pagination

> Deterministic ordering requires index support

**Notes**

* Sort without index = blocking
* Prefer range-based pagination

<details>
<summary>Examples (Node.js)</summary>

```js
// Sort + limit
await db.collection('users')
  .find({})
  .sort({ createdAt: -1 }) // 1 | -1
  .limit(10)
  .toArray();

// Pagination with skip (anti-pattern at scale)
await db.collection('users')
  .find({})
  .skip(20)
  .limit(10)
  .toArray();
```

</details>

---

## Update Operators

> In-place mutations without document replacement

**Categories**

* Field: `$set`, `$unset`, `$inc`
* Array: `$push`, `$pull`, `$addToSet`
* Conditional: `$min`, `$max`

<details>
<summary>Examples (Node.js)</summary>

```js
// Field update
await db.collection('users').updateOne(
  { _id: 1 },
  { $set: { status: 'active' }, $inc: { loginCount: 1 } }
);

// Array update
await db.collection('users').updateOne(
  { _id: 1 },
  { $addToSet: { roles: 'admin' } }
);
```

</details>

---

## Replace vs Update

> Replace overwrites document; update mutates fields

**Differences**

* `replaceOne` removes unspecified fields
* Update operators preserve others

<details>
<summary>Examples (Node.js)</summary>

```js
// replaceOne (full overwrite)
await db.collection('users').replaceOne(
  { _id: 1 },
  { name: 'A', status: 'active' }
);

// updateOne (partial)
await db.collection('users').updateOne(
  { _id: 1 },
  { $set: { status: 'inactive' } }
);
```

</details>

---

## Delete Semantics

> Deletes are immediate and non-recoverable

**Notes**

* No soft delete by default
* Indexes affect delete performance

<details>
<summary>Examples (Node.js)</summary>

```js
// deleteOne
await db.collection('users').deleteOne({ _id: 1 });

// deleteMany
await db.collection('users').deleteMany({ status: 'inactive' });
```

</details>

---

## Bulk Operations

> High-throughput batched writes

**Characteristics**

* Ordered or unordered
* Partial success possible

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').bulkWrite([
  { insertOne: { document: { _id: 10 } } },
  { updateOne: {
      filter: { _id: 2 },
      update: { $set: { status: 'active' } },
      upsert: true // boolean
  }},
  { deleteOne: { filter: { _id: 3 } } }
], {
  ordered: false // boolean
});
```

</details>

---

## Atomicity Guarantees

> Atomic at single-document level; multi-doc via transactions

**Guarantees**

* Single-document writes are atomic
* Isolation across documents not guaranteed

<details>
<summary>Examples (Node.js)</summary>

```js
// Atomic single-document update
await db.collection('accounts').updateOne(
  { _id: 1, balance: { $gte: 100 } },
  { $inc: { balance: -100 } }
);

// Multi-document transaction
const session = client.startSession();
await session.withTransaction(async () => {
  await db.collection('a').updateOne({ _id: 1 }, { $inc: { v: -1 } }, { session });
  await db.collection('b').updateOne({ _id: 1 }, { $inc: { v: 1 } }, { session });
});
```

</details>

---

## Upserts

> Insert-or-update semantics controlled explicitly

**Facts**

* Disabled by default
* Insert path applies update operators

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').updateOne(
  { email: 'a@x.com' },
  { $set: { name: 'A' }, $setOnInsert: { createdAt: new Date() } },
  { upsert: true } // boolean
);
```

</details>

---

## findOneAnd* Operations

> Read-modify-write with server-side atomicity

**Variants**

* `findOneAndUpdate`
* `findOneAndReplace`
* `findOneAndDelete`

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('jobs').findOneAndUpdate(
  { status: 'pending' },
  { $set: { status: 'processing' } },
  {
    sort: { priority: -1 },     // sort order
    returnDocument: 'after',    // 'before' | 'after'
    projection: { _id: 1 }      // object
  }
);
```

</details>

---

## Write Concern

> Durability guarantees for writes

**Levels**

* `w: 1` (default)
* `w: 'majority'`
* Journaling control

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').insertOne(
  { _id: 1 },
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

## Read Concern and Read Preference

> Consistency and replica targeting for reads

**Read Concern**

* `local`, `majority`, `snapshot`

**Read Preference**

* `primary`, `secondary`, `nearest`

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').find(
  {},
  {
    readConcern: { level: 'majority' }, // 'local' | 'majority' | 'snapshot'
    readPreference: 'secondaryPreferred' // string
  }
).toArray();
```

</details>

---

## Collation

> Locale-aware string comparison

**Uses**

* Case-insensitive queries
* Language-specific sorting

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').find(
  { name: 'alex' },
  {
    collation: {
      locale: 'en',
      strength: 2 // case-insensitive
    }
  }
).toArray();
```

</details>

---

## Hints and Explain

> Query planner control and introspection

<details>
<summary>Examples (Node.js)</summary>

```js
// Force index usage
await db.collection('users')
  .find({ email: 'a@x.com' })
  .hint({ email: 1 })
  .toArray();

// Explain plan
await db.collection('users')
  .find({ age: { $gt: 30 } })
  .explain('executionStats'); // 'queryPlanner' | 'executionStats'
```

</details>

---

## Notes

* CRUD semantics are **simple but sharp-edged**
* Indexes dominate read & write behavior
* Most bugs come from misunderstood filters

## Caveats

* Updates without operators cause replacement
* Skip-based pagination does not scale
* Bulk writes can partially succeed
