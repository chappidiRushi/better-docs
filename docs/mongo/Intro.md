---
id: mongodb-database-system
title: MongoDB as a Database System
sidebar_position: 2
-------------------

# 2. MongoDB as a Database System

> High-level architecture, philosophy, and tradeoffs of MongoDB as a modern database

---

## MongoDB Philosophy and Design Goals

> Flexible schema, horizontal scalability, developer productivity, operational simplicity

**Core ideas**

* JSON-like document model (BSON)
* Scale-out via sharding, not scale-up
* Strong defaults, tunable guarantees
* Application-driven schema

<details>
<summary>Examples</summary>

```js
// Schema flexibility (same collection, different shapes)
db.users.insertMany([
  { _id: 1, name: 'A', email: 'a@x.com' },
  { _id: 2, name: 'B', phones: ['+1', '+91'], meta: { verified: true } }
]);
```

</details>

---

## MongoDB vs Relational Databases

> Document-first vs relation-first, runtime joins vs pre-embedded data

**Key differences**

* No fixed schema / migrations optional
* Joins via `$lookup` (not default access path)
* Denormalization favored
* No cross-document ACID by default (pre-4.0)

<details>
<summary>Examples</summary>

```js
// Embedded model (MongoDB preferred)
{
  _id: ObjectId(),
  orderId: 'O1',
  user: { id: 10, name: 'Alex' },
  items: [{ sku: 'P1', qty: 2 }]
}

// Relational-style lookup
 db.orders.aggregate([
  { $lookup: {
      from: 'users',
      localField: 'userId',
      foreignField: '_id',
      as: 'user'
  }}
]);
```

</details>

---

## Document Model Tradeoffs

> Read-optimized, write-amplified, bounded document size

**Pros**

* Fewer round-trips
* Atomic single-document updates
* Natural object mapping

**Cons**

* 16MB document limit
* Data duplication
* Complex updates for deeply nested arrays

<details>
<summary>Examples</summary>

```js
// Atomic update on embedded document
db.accounts.updateOne(
  { _id: 1 },
  { $inc: { 'balance.available': -100 } }
);

// Risky unbounded array growth
{ _id: 1, logs: [ /* unbounded */ ] }
```

</details>

---

## Use Cases and Anti-Use Cases

> Best for operational workloads, not analytical-heavy joins

**Good fit**

* Event stores
* Content management
* User profiles
* IoT / time-series

**Poor fit**

* Heavy ad-hoc joins
* Highly normalized financial systems
* Cross-row aggregate constraints

<details>
<summary>Examples</summary>

```js
// Event-style workload
db.events.insertOne({
  ts: ISODate(),
  type: 'click',
  userId: 123,
  ctx: { page: '/home' }
});

// Anti-pattern: relational ledger
// Requires multi-document consistency + joins
```

</details>

---

## CAP and Consistency Guarantees

> CP system by default, tunable consistency via read/write concerns

**Defaults**

* Writes: acknowledged by primary
* Reads: primary
* Replication is async

**Tunable knobs**

* `writeConcern`
* `readConcern`
* `readPreference`

<details>
<summary>Examples</summary>

```js
// Stronger write durability
db.orders.insertOne(
  { orderId: 1 },
  { writeConcern: { w: 'majority', j: true, wtimeout: 5000 } }
);

// Snapshot read (transactions / causal consistency)
db.getMongo().startSession({
  defaultTransactionOptions: { readConcern: { level: 'snapshot' } }
});
```

</details>

---

## Editions, Licensing, and Versioning

> Open-core distribution with SSPL, rapid release cadence

**Editions**

* Community (free)
* Enterprise (commercial)
* Atlas (managed)

**Licensing**

* SSPL ≥ 4.0
* Drivers: Apache 2.0

**Versioning**

* Semantic-ish: MAJOR.MINOR
* Rapid feature deprecation
* FCV (Feature Compatibility Version)

<details>
<summary>Examples</summary>

```js
// Check server version
db.version();

// Check FCV
db.adminCommand({ getParameter: 1, featureCompatibilityVersion: 1 });

// Set FCV (downgrade-safe)
db.adminCommand({
  setFeatureCompatibilityVersion: '7.0' // string
});
```

</details>

---

## Notes

* MongoDB optimizes for **developer velocity over strict normalization**
* Data modeling decisions are workload-driven
* Consistency is explicit, not implicit

## Caveats

* Over-embedding causes document bloat
* `$lookup` is not a free join
* Misconfigured concerns lead to silent data loss
