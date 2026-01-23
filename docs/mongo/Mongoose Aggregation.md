---

id: mongoose-aggregation
title: Mongoose Aggregation
sidebar_position: 32
--------------------

# 32. Mongoose Aggregation

> Thin wrapper over MongoDB Aggregation Framework with schema-aware casting

---

## Aggregation Wrapper

> How Mongoose exposes MongoDB aggregation pipelines

**Interview points**

* `Model.aggregate()` returns an Aggregation object
* Mostly pass-through to MongoDB
* Adds minimal abstraction (casting, middleware)

<details>
<summary>Examples (Node.js)</summary>

```js
// Basic aggregation
const result = await Order.aggregate([
  { $match: { status: 'paid' } },
  { $group: { _id: '$userId', total: { $sum: '$amount' } } }
]);

// Aggregation object (lazy)
const agg = Order.aggregate();
agg.match({ status: 'paid' });
agg.group({ _id: '$userId', total: { $sum: '$amount' } });

await agg.exec(); // execution
```

</details>

---

## Pipeline Casting

> Automatic casting of pipeline values using schema metadata

**Interview points**

* Only applies to `$match`
* Driven by SchemaTypes
* Can be disabled

<details>
<summary>Examples (Node.js)</summary>

```js
// Schema
const schema = new Schema({
  userId: Schema.Types.ObjectId,
  createdAt: Date,
  amount: Number
});

// String → ObjectId cast
await Order.aggregate([
  { $match: { userId: '64f0c1...' } }
]);

// String → Date cast
await Order.aggregate([
  { $match: { createdAt: '2024-01-01' } }
]);

// Disable casting
Order.aggregate([{ $match: { amount: '100' } }]).option({ cast: false });
```

</details>

---

## Performance Considerations

> Cost model and optimization constraints

**Interview points**

* Runs entirely on MongoDB server
* Bypasses Mongoose middleware (mostly)
* Memory limits apply (100MB unless `allowDiskUse`)

<details>
<summary>Examples (Node.js)</summary>

```js
// Allow disk use
await Order.aggregate([
  { $sort: { createdAt: -1 } },
  { $group: { _id: '$userId', total: { $sum: '$amount' } } }
]).option({ allowDiskUse: true });

// Index-aware $match should come early
await Order.aggregate([
  { $match: { status: 'paid' } }, // uses index
  { $project: { userId: 1, amount: 1 } }
]);
```

</details>

---

## Streaming Aggregations

> Processing aggregation results incrementally

**Interview points**

* Uses MongoDB cursors
* Prevents loading full result set into memory
* Useful for large pipelines

<details>
<summary>Examples (Node.js)</summary>

```js
// Cursor-based aggregation
const cursor = Order.aggregate([
  { $match: { status: 'paid' } },
  { $group: { _id: '$userId', total: { $sum: '$amount' } } }
]).cursor({ batchSize: 100 }).exec();

for await (const doc of cursor) {
  process(doc);
}

// Stream API
Order.aggregate([...]).cursor().on('data', doc => {
  handle(doc);
});
```

</details>

---

## Notes

* Aggregation results are plain JS objects
* No document middleware or validation
* Preferred for analytics and reporting

## Caveats

* Casting is limited compared to queries
* Complex pipelines are harder to debug
* Aggregations ignore default projections
