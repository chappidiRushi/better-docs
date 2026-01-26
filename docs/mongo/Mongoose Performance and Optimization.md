---

id: mongoose-performance-optimization
title: Performance and Optimization
sidebar_position: 39
--------------------

# Performance and Optimization

> Query efficiency, population costs, throughput patterns, memory control, and profiling techniques

---

## 1. Lean Queries

> Skip document hydration for maximum read performance

```js
Query.prototype.lean(options?)   // → Query<Array|Object>
```

<details>
<summary>Examples</summary>

```js
// basic lean
await User.find({ active: true })
  .lean();                        // boolean | object

// lean with options
await User.findById(id)
  .lean({
    virtuals: false,              // boolean
    getters: false,               // boolean
    defaults: false               // boolean
  });
```

</details>

---

## 2. Population Performance

> Population introduces extra queries and memory overhead

```js
.populate(path, select?, options?)
```

<details>
<summary>Examples</summary>

```js
// naive populate
await Post.find()
  .populate('author');            // string | object

// optimized populate
await Post.find()
  .populate({
    path: 'author',               // string
    select: 'name email',         // string | object
    options: { lean: true }       // object
  });

// manual batching alternative
const posts = await Post.find().lean();
const users = await User.find({ _id: { $in: posts.map(p => p.author) } }).lean();
```

</details>

---

## 3. High-Throughput Patterns

> Patterns for write-heavy or read-heavy systems

```js
bulkWrite | insertMany | updateMany
```

<details>
<summary>Examples</summary>

```js
// bulkWrite
await User.bulkWrite([
  { insertOne: { document: { email: 'a@test.com' } } },
  { updateOne: {
      filter: { email: 'b@test.com' },
      update: { $inc: { score: 1 } },
      upsert: true
  }}
], {
  ordered: false                 // boolean
});

// insertMany
await Log.insertMany(logs, {
  ordered: false,                // boolean
  lean: true                     // boolean
});
```

</details>

---

## 4. Memory Usage

> Control memory footprint during large operations

```js
cursor | batchSize | stream
```

<details>
<summary>Examples</summary>

```js
// cursor iteration
const cursor = User.find({}).lean().cursor({
  batchSize: 500                 // number
});

for await (const doc of cursor) {
  // process doc
}

// limit document size
await User.find({}, 'email name') // projection
  .lean();
```

</details>

---

## 5. Profiling

> Identify slow queries and bottlenecks

```js
explain | mongoose.set('debug')
```

<details>
<summary>Examples</summary>

```js
// explain plan
await User.find({ email })
  .explain('executionStats');     // 'queryPlanner' | 'executionStats'

// mongoose debug
mongoose.set('debug', function (coll, method, query, doc) {
  console.log(coll, method, query);
});

// MongoDB profiler (shell)
db.setProfilingLevel(1, { slowms: 50 });
```

</details>

---

## Notes

* Lean queries can be 2–5x faster
* Population scales poorly with large datasets
* Bulk operations reduce network round-trips

## Caveats

* Lean queries bypass middleware and virtuals
* Overusing population leads to N+1 problems
* Profiling in production must be time-boxed
