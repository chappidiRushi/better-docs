---

id: mongodb-data-modeling-patterns
title: Data Modeling Patterns
sidebar_position: 15
--------------------

# Data Modeling Patterns

> Proven schema design patterns for performance, scalability, and maintainability

---

## Embedding Patterns

> Store related data inside a single document

**Interview points**

* Optimizes read performance
* Single-document atomicity
* Document size limit applies

<details>
<summary>Examples (Node.js)</summary>

```js
// Embedded address inside user
await db.collection('users').insertOne({
  _id: 1,
  name: 'A',
  addresses: [
    { city: 'NY', zip: '10001' },
    { city: 'SF', zip: '94101' }
  ]
});
```

</details>

---

## Referencing Patterns

> Store relationships using document references

**Interview points**

* Reduces document size
* Requires joins ($lookup or app-side)
* Suitable for high-cardinality relations

<details>
<summary>Examples (Node.js)</summary>

```js
// User reference in orders
await db.collection('orders').insertOne({
  _id: 101,
  userId: 1, // reference
  amount: 500
});

// Application-side join
const order = await db.collection('orders').findOne({ _id: 101 });
const user = await db.collection('users').findOne({ _id: order.userId });
```

</details>

---

## Bucket Pattern

> Group multiple related records into buckets

**Interview points**

* Reduces document count
* Common for time-series
* Tradeoff between update and read cost

<details>
<summary>Examples (Node.js)</summary>

```js
// Bucketed sensor readings
await db.collection('metrics').updateOne(
  { sensorId: 's1', bucket: '2025-01-01' },
  {
    $push: { values: { ts: Date.now(), v: 42 } },
    $inc: { count: 1 }
  },
  { upsert: true }
);
```

</details>

---

## Polymorphic Pattern

> Store different document shapes in one collection

**Interview points**

* Flexible schema
* Requires discriminator field
* Indexing must account for shape variance

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('assets').insertMany([
  { type: 'car', wheels: 4, brand: 'BMW' },
  { type: 'bike', wheels: 2, brand: 'Yamaha' }
]);

// Query specific subtype
await db.collection('assets').find({ type: 'car' }).toArray();
```

</details>

---

## Tree Structures

> Represent hierarchical data

**Interview points**

* Parent reference, materialized path, or nested set
* Tradeoff between read and write complexity

<details>
<summary>Examples (Node.js)</summary>

```js
// Parent reference model
await db.collection('categories').insertMany([
  { _id: 1, name: 'Electronics', parentId: null },
  { _id: 2, name: 'Laptops', parentId: 1 }
]);

// Fetch children
await db.collection('categories').find({ parentId: 1 }).toArray();
```

</details>

---

## Time-Series Modeling

> Optimized design for time-based data

**Interview points**

* Append-only workload
* Leverages bucket pattern
* Native time-series collections available

<details>
<summary>Examples (Node.js)</summary>

```js
// Time-series collection
await db.createCollection('events', {
  timeseries: {
    timeField: 'ts',
    metaField: 'deviceId',
    granularity: 'seconds' // seconds | minutes | hours
  }
});
```

</details>

---

## Event Sourcing

> Persist state changes as immutable events

**Interview points**

* Append-only writes
* Rebuild state by replaying events
* Strong auditability

<details>
<summary>Examples (Node.js)</summary>

```js
// Append event
await db.collection('order_events').insertOne({
  orderId: 1,
  type: 'ORDER_PAID',
  payload: { amount: 500 },
  ts: new Date()
});

// Replay events
await db.collection('order_events')
  .find({ orderId: 1 })
  .sort({ ts: 1 })
  .toArray();
```

</details>

---

## Notes

* Schema design dominates performance
* Prefer embedding unless growth or cardinality forbids

## Caveats

* Over-embedding causes document growth issues
* Excessive referencing increases query cost
* Polymorphism complicates indexing
