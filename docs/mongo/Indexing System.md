---

id: mongodb-indexing-system
title: Indexing System
sidebar_position: 7
-------------------

#  Indexing System

> How MongoDB indexes are built, stored, and used by the query planner

---

## Index Internals

> Indexes are ordered data structures mapping keys to document locations

**Interview points**

* Implemented as B-Trees (WiredTiger)
* Store key + record pointer
* Separate from collection data

<details>
<summary>Examples (Node.js)</summary>

```js
// Create basic index
await db.collection('users').createIndex({ email: 1 }); // 1 | -1

// Inspect indexes
await db.collection('users').indexes();
```

</details>

---

## Single and Compound Indexes

> Single-field vs multi-field ordered indexes

**Interview points**

* Compound index order matters
* Supports equality + range queries

<details>
<summary>Examples (Node.js)</summary>

```js
// Single-field index
await db.collection('users').createIndex({ age: 1 });

// Compound index
await db.collection('users').createIndex({ status: 1, createdAt: -1 });
```

</details>

---

## Multikey Indexes

> Indexes on array fields

**Interview points**

* One index entry per array element
* Cannot compound on multiple array fields

<details>
<summary>Examples (Node.js)</summary>

```js
// Array field index
await db.collection('users').createIndex({ roles: 1 });

// Query uses multikey index
await db.collection('users').find({ roles: 'admin' }).toArray();
```

</details>

---

## Index Prefix Rules

> Leftmost prefix determines usability of compound indexes

**Interview points**

* Queries must include leading fields
* Skipping breaks index usage

<details>
<summary>Examples (Node.js)</summary>

```js
// Index
await db.collection('orders').createIndex({ userId: 1, createdAt: 1, status: 1 });

// Uses index (prefix)
await db.collection('orders').find({ userId: 1, createdAt: { $gt: new Date() } });

// Does NOT use index (skips prefix)
await db.collection('orders').find({ status: 'open' });
```

</details>

---

## Text Indexes

> Full-text search with language-specific tokenization

**Interview points**

* One text index per collection
* Uses inverted index

<details>
<summary>Examples (Node.js)</summary>

```js
// Create text index
await db.collection('articles').createIndex({ title: 'text', body: 'text' });

// Text search
await db.collection('articles').find({ $text: { $search: 'mongodb index' } }).toArray();
```

</details>

---

## Geospatial Indexes

> Spatial queries on coordinate data

**Interview points**

* `2dsphere` for Earth-like geometry
* Supports $near, $geoWithin

<details>
<summary>Examples (Node.js)</summary>

```js
// 2dsphere index
await db.collection('places').createIndex({ location: '2dsphere' });

// Geo query
await db.collection('places').find({
  location: {
    $near: {
      $geometry: { type: 'Point', coordinates: [77.6, 12.9] },
      $maxDistance: 5000 // meters
    }
  }
}).toArray();
```

</details>

---

## Partial and Sparse Indexes

> Index only a subset of documents

**Interview points**

* Sparse: only documents with field
* Partial: conditional filter expression

<details>
<summary>Examples (Node.js)</summary>

```js
// Sparse index
await db.collection('users').createIndex({ email: 1 }, { sparse: true });

// Partial index
await db.collection('users').createIndex(
  { status: 1 },
  { partialFilterExpression: { status: 'active' } }
);
```

</details>

---

## TTL Indexes

> Automatic document expiration

**Interview points**

* Background deletion
* Not precise to the second

<details>
<summary>Examples (Node.js)</summary>

```js
// TTL index
await db.collection('sessions').createIndex(
  { expiresAt: 1 },
  { expireAfterSeconds: 0 } // seconds
);
```

</details>

---

## Index Build Strategies

> How indexes are created without blocking

**Interview points**

* Foreground vs background (pre-4.2)
* Online index builds

<details>
<summary>Examples (Node.js)</summary>

```js
// Online index build (default)
await db.collection('users').createIndex({ lastLogin: -1 });

// Rolling index build strategy
// Build on secondaries → step up primary
```

</details>

---

## Index Performance Tuning

> Designing indexes for planner efficiency

**Interview points**

* Match query patterns
* Avoid unused indexes
* Monitor with explain

<details>
<summary>Examples (Node.js)</summary>

```js
// Force index usage
await db.collection('users')
  .find({ email: 'a@x.com' })
  .hint({ email: 1 })
  .explain('executionStats');

// Drop unused index
await db.collection('users').dropIndex({ age: 1 });
```

</details>

---

## Notes

* Indexes trade write performance for read speed
* Planner decisions depend entirely on indexes

## Caveats

* Over-indexing slows writes
* Wrong index order negates usefulness
* Multikey indexes have strict limitations
