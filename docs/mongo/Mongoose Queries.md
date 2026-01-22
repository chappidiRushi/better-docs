---

id: mongoose-queries
title: Mongoose Queries and Query Builder
sidebar_position: 30
--------------------

# 30. Mongoose Queries and Query Builder

> Fluent query construction, casting, and execution layer over the MongoDB driver

---

## Query Construction

> Building query objects using the chainable Query API

**Interview points**

* Queries are **lazy** until executed
* Chainable and immutable until execution
* Returns a `Query` object, not a Promise

<details>
<summary>Examples (Node.js)</summary>

```js
// Basic construction
const q = User.find({ age: { $gte: 18 } })
  .where('status').equals('active')
  .limit(10)
  .skip(20);

// Query is NOT executed yet

await q.exec(); // execution trigger

// One-liner
await User.find({ role: 'admin' }).select('email').exec();
```

</details>

---

## Casting and Coercion

> Automatic conversion of query values to SchemaTypes

**Interview points**

* Happens before sending query to MongoDB
* Driven by SchemaTypes
* Can throw `CastError`

<details>
<summary>Examples (Node.js)</summary>

```js
// Schema
const schema = new Schema({
  age: Number,
  active: Boolean,
  createdAt: Date,
});

// String → Number
await User.find({ age: '42' }); // cast to 42

// String → Boolean
await User.find({ active: 'true' }); // cast to true

// Invalid cast
await User.find({ age: 'abc' }); // throws CastError

// Disable casting (rare)
User.find({ age: '42' }).cast(false);
```

</details>

---

## Execution Lifecycle

> Transition from Query object to MongoDB operation

**Interview points**

* Execution methods: `exec()`, `then()`, `await`
* Middleware applied before execution
* Results hydrated into documents (unless `lean`)

<details>
<summary>Examples (Node.js)</summary>

```js
const query = User.findOne({ email });

// Middleware NOT yet run

const doc = await query.exec(); // execution starts

// Equivalent executions
await User.findById(id);
await User.findById(id).then(d => d);

// Re-execution not allowed
await query.exec(); // ❌ throws error
```

</details>

---

## Projection and Sorting

> Controlling returned fields and result order

**Interview points**

* Projection reduces payload size
* Sorting impacts index usage
* Exclusion overrides inclusion rules

<details>
<summary>Examples (Node.js)</summary>

```js
// Projection (inclusion)
await User.find()
  .select('email name -_id'); // include + exclude

// Projection (object form)
await User.find().select({ email: 1, name: 1 });

// Sorting
await User.find()
  .sort({ createdAt: -1, email: 1 })
  .limit(20);

// Index-aware sort
// Requires index on { createdAt: -1, email: 1 }
```

</details>

---

## Cursor Queries

> Streaming large result sets using MongoDB cursors

**Interview points**

* Avoids loading full result set into memory
* Useful for batch processing
* Uses Node.js streams or async iterators

<details>
<summary>Examples (Node.js)</summary>

```js
// Cursor
const cursor = User.find({ active: true }).cursor();

for await (const doc of cursor) {
  process(doc);
}

// With batch size
User.find().cursor({ batchSize: 100 });

// Stream API
User.find().cursor().on('data', doc => {
  handle(doc);
});
```

</details>

---

## Explain Plans

> Inspecting how MongoDB executes a query

**Interview points**

* Delegates to MongoDB `explain`
* Used for index validation
* Does NOT return documents

<details>
<summary>Examples (Node.js)</summary>

```js
// Default verbosity
await User.find({ email: 'a@b.com' }).explain();

// Execution stats
await User.find({ age: { $gt: 30 } })
  .explain('executionStats');

// Query planner only
await User.find({ active: true })
  .explain('queryPlanner');
```

</details>

---

## Notes

* Queries are immutable once executed
* `lean()` skips hydration and middleware
* Query helpers extend the Query prototype

## Caveats

* Reusing executed queries throws errors
* Implicit casting can hide bugs
* Sorting without indexes causes in-memory sorts
