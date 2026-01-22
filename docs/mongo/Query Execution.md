---

id: mongodb-query-execution-engine
title: Query Execution Engine
sidebar_position: 6
-------------------

# Query Execution Engine

> How MongoDB parses, plans, executes, and optimizes queries

---

## Query Parsing

> Converts client queries into an internal canonical representation

**Interview points**

* Syntax + semantic validation
* Normalizes query shape
* Rejects invalid operators early

<details>
<summary>Examples (Node.js)</summary>

```js
// Parsed into internal match expression tree
await db.collection('users').find({
  age: { $gte: 18 },
  status: 'active'
}).toArray();

// Invalid operator → parse-time error
await db.collection('users').find({ age: { $foo: 10 } }).toArray();
```

</details>

---

## Query Planner

> Chooses the most efficient execution strategy

**Interview points**

* Generates multiple candidate plans
* Ranks plans using cost model
* Uses available indexes

<details>
<summary>Examples (Node.js)</summary>

```js
// Planner evaluates index options
await db.collection('users')
  .find({ email: 'a@x.com', status: 'active' })
  .explain('queryPlanner');
```

</details>

---

## Execution Plans

> Concrete plan tree executed by the engine

**Interview points**

* Stage-based execution (IXSCAN, FETCH, SORT)
* Blocking vs streaming stages
* Short-circuiting on limits

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users')
  .find({ age: { $gt: 30 } })
  .sort({ age: 1 })
  .limit(10)
  .explain('executionStats');
```

</details>

---

## Covered Queries

> Queries satisfied entirely by an index

**Interview points**

* No document fetch (no FETCH stage)
* Requires index covers filter + projection
* Fastest possible read path

<details>
<summary>Examples (Node.js)</summary>

```js
// Create covering index
await db.collection('users').createIndex({ email: 1, status: 1 });

// Covered query
await db.collection('users')
  .find(
    { email: 'a@x.com' },
    { projection: { email: 1, status: 1, _id: 0 } }
  )
  .explain('executionStats');
```

</details>

---

## Cursor Behavior

> Server-side cursor streams results in batches

**Interview points**

* Default batch size (~101)
* Cursor timeout (10 min idle)
* Cursor pinned during getMore

<details>
<summary>Examples (Node.js)</summary>

```js
const cursor = db.collection('users')
  .find({})
  .batchSize(50); // number

while (await cursor.hasNext()) {
  await cursor.next();
}
```

</details>

---

## Query Cache

> Caches winning plans by query shape

**Interview points**

* Keyed by query shape, not values
* Re-plans on performance degradation
* Invalidated on index changes

<details>
<summary>Examples (Node.js)</summary>

```js
// Same shape, different values → cached plan
await db.collection('users').find({ age: { $gt: 20 } }).toArray();
await db.collection('users').find({ age: { $gt: 40 } }).toArray();

// Clear plan cache
await db.collection('users').getPlanCache().clear();
```

</details>

---

## Explain Plans

> Introspection into planner and executor behavior

**Interview points**

* `queryPlanner`: chosen plan only
* `executionStats`: runtime metrics
* `allPlansExecution`: compares candidates

<details>
<summary>Examples (Node.js)</summary>

```js
await db.collection('users')
  .find({ age: { $gte: 18 } })
  .explain('allPlansExecution');
```

</details>

---

## Notes

* Query shape stability improves cache reuse
* Index design directly controls planner output
* Explain is mandatory for performance debugging

## Caveats

* Cached plans can become suboptimal
* Blocking stages impact memory
* Missing projections break covered queries
