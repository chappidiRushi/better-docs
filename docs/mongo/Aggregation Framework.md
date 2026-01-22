---

id: mongodb-aggregation-framework
title: Aggregation Framework
sidebar_position: 8
-------------------

# 8. Aggregation Framework

> Server-side data processing pipeline for analytics and transformations

---

## Pipeline Architecture

> Aggregation is a staged, ordered pipeline where each stage transforms documents

**Interview points**

* Documents flow stage by stage
* Output of one stage is input to next
* Declarative, not procedural

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').aggregate([
  { $match: { status: 'paid' } },
  { $group: { _id: '$userId', total: { $sum: '$amount' } } }
]).toArray();
```

</details>

---

## Pipeline Execution

> Pipelines execute inside the query engine with possible index usage

**Interview points**

* Early stages can use indexes
* Blocking vs streaming stages
* Executes on primary or shards

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').aggregate([
  { $match: { createdAt: { $gte: new Date('2024-01-01') } } }, // indexable
  { $sort: { createdAt: -1 } }, // may spill to disk
  { $limit: 10 }
]).toArray();
```

</details>

---

## Core Stages

> Fundamental transformation and control stages

**Interview points**

* $match, $project, $group, $sort, $limit, $skip
* Order impacts performance

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users').aggregate([
  { $match: { active: true } },
  { $project: { email: 1, age: 1 } },
  { $sort: { age: -1 } },
  { $limit: 5 }
]).toArray();
```

</details>

---

## Expressions and Operators

> Functional expressions used inside stages

**Interview points**

* Arithmetic, conditional, array, date operators
* Used inside $project, $group, $match

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').aggregate([
  {
    $project: {
      totalWithTax: { $multiply: ['$amount', 1.18] }, // number
      isLarge: { $gt: ['$amount', 1000] },             // boolean
      year: { $year: '$createdAt' }                    // date
    }
  }
]).toArray();
```

</details>

---

## $lookup and Joins

> Left outer joins across collections

**Interview points**

* Performed at query time
* Can be pipeline-based
* Expensive without indexes

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('orders').aggregate([
  {
    $lookup: {
      from: 'users',
      localField: 'userId',
      foreignField: '_id',
      as: 'user'
    }
  },
  { $unwind: '$user' }
]).toArray();
```

</details>

---

## Memory Limits

> Resource constraints during aggregation execution

**Interview points**

* 100MB memory limit per stage
* Disk spill with allowDiskUse

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('events').aggregate([
  { $group: { _id: '$type', count: { $sum: 1 } } }
], {
  allowDiskUse: true // boolean
}).toArray();
```

</details>

---

## Aggregation Optimization

> Techniques to improve pipeline performance

**Interview points**

* Push $match early
* Reduce document shape
* Avoid unnecessary $lookup

<details>
<summary>Examples (Node.js)</summary>

```js
// Optimized pipeline
await db.collection('orders').aggregate([
  { $match: { status: 'paid' } },
  { $project: { amount: 1, userId: 1 } },
  { $group: { _id: '$userId', total: { $sum: '$amount' } } }
]).explain('executionStats');
```

</details>

---

## Aggregation vs MapReduce

> Modern aggregation replaces MapReduce

**Interview points**

* Aggregation is faster and optimized
* MapReduce is deprecated
* Aggregation is type-safe and index-aware

<details>
<summary>Examples (Node.js)</summary>

```js
// Aggregation (preferred)
await db.collection('logs').aggregate([
  { $group: { _id: '$level', count: { $sum: 1 } } }
]).toArray();

// MapReduce (legacy)
await db.collection('logs').mapReduce(
  function () { emit(this.level, 1); },
  function (key, values) { return Array.sum(values); }
);
```

</details>

---

## Notes

* Aggregation runs close to data
* Indexes significantly impact performance

## Caveats

* Large $lookup can be expensive
* Blocking stages increase memory usage
* Poor stage order kills performance
