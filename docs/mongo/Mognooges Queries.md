---

id: mongoose-queries
title: Mongoose Queries – Complete Reference
sidebar_position: 1
-------------------

# Mongoose Queries – Complete Reference

> Exhaustive reference for **query construction, execution, options, cursors, and performance tuning** in Mongoose.
> Assumes **hardcore / senior-level MongoDB & Mongoose usage**.

---

## Model Query Methods

```js
Model.find(filter?, projection?, options?)       // returns Query<Array<Doc>>
Model.findOne(filter?, projection?, options?)    // returns Query<Doc|null>
Model.findById(id, projection?, options?)        // id | string | ObjectId
Model.countDocuments(filter?, options?)          // accurate count
Model.estimatedDocumentCount(options?)           // fast, approximate
```

<details>
<summary>Examples</summary>

```js
await User.find({ isActive: true });
await User.findOne({ email });
await User.findById(userId);
await User.countDocuments({ role: 'admin' });
await User.estimatedDocumentCount();
```

</details>

---

## Query Filters (Conditions)

```js
// comparison
{ age: { $gt: 18, $lte: 60 } }

// logical
{ $and: [{ a: 1 }, { b: 2 }] }
{ $or: [{ a: 1 }, { b: 2 }] }
{ $nor: [{ a: 1 }, { b: 2 }] }
{ $not: { a: 1 } }

// element
{ field: { $exists: true, $type: 'string' } }

// array
{ tags: { $in: ['a', 'b'] } }
{ tags: { $all: ['a', 'b'] } }
{ tags: { $size: 3 } }

// regex
{ name: /john/i }
```

<details>
<summary>Examples</summary>

```js
await User.find({ age: { $gte: 18, $lt: 30 } });
await User.find({ $or: [{ role: 'admin' }, { isActive: true }] });
await User.find({ tags: { $in: ['node', 'mongo'] } });
```

</details>

---

## Projection (Field Selection)

```js
// include
{ name: 1, email: 1 }

// exclude
{ password: 0 }

// string syntax
'name email -_id'
```

<details>
<summary>Examples</summary>

```js
await User.find({}, 'name email');
await User.find({}, { name: 1, email: 1 });
await User.find({}, { password: 0, deletedAt: 0 });
```

</details>

---

## Query Modifiers

```js
query.sort(sort)            // object | string
query.limit(n)              // number
query.skip(n)               // number
query.collation(options)    // object
query.hint(index)           // object | string
```

<details>
<summary>Examples</summary>

```js
await User.find()
  .sort({ created_at: -1 })
  .skip(20)
  .limit(10)
  .collation({ locale: 'en', strength: 2 })
  .hint({ email: 1 });
```

</details>

---

## Lean Queries

```js
query.lean(options?)        // boolean | object
```

<details>
<summary>Examples</summary>

```js
await User.find().lean();
await User.find().lean({ virtuals: false, getters: false });
```

</details>

---

## Populate

```js
query.populate(path | options)
```

<details>
<summary>Examples</summary>

```js
await User.find()
  .populate('organizationId');

await User.find()
  .populate({
    path: 'organizationId',
    select: 'name',
    match: { isActive: true },
    options: { lean: true }
  });
```

</details>

---

## Query Execution

```js
query.exec()                // Promise
await query                 // implicit exec
```

<details>
<summary>Examples</summary>

```js
const q = User.find({ isActive: true });
await q.exec();
await q; // same
```

</details>

---

## Cursors & Streaming

```js
query.cursor(options?)      // QueryCursor
```

<details>
<summary>Examples</summary>

```js
for await (const doc of User.find().cursor()) {
  console.log(doc._id);
}
```

</details>

---

## Pagination Patterns

```js
// offset-based
query.skip(page * limit).limit(limit)

// cursor-based
{ _id: { $gt: lastId } }
```

<details>
<summary>Examples</summary>

```js
// offset
await User.find().skip(100).limit(20);

// cursor
await User.find({ _id: { $gt: lastId } })
  .sort({ _id: 1 })
  .limit(20);
```

</details>

---

## Transactions & Sessions

```js
query.session(session)
```

<details>
<summary>Examples</summary>

```js
const session = await mongoose.startSession();
await session.withTransaction(async () => {
  await User.find({ isActive: true }).session(session);
});
session.endSession();
```

</details>

---

## Explain & Debugging

```js
query.explain(verbosity?)   // 'queryPlanner' | 'executionStats' | 'allPlansExecution'
```

<details>
<summary>Examples</summary>

```js
await User.find({ email })
  .hint({ email: 1 })
  .explain('executionStats');
```

</details>

---

## Error Scenarios

```js
CastError
ValidationError
MongoServerError
```

<details>
<summary>Examples</summary>

```js
try {
  await User.findById('invalid');
} catch (err) {
  if (err.name === 'CastError') {
    // handle
  }
}
```

</details>

---

## Aggregation Framework

```js
Model.aggregate(pipeline, options?)   // returns Aggregation
```

<details>
<summary>Examples</summary>

```js
// basic pipeline
await User.aggregate([
  { $match: { isActive: true } },
  { $group: { _id: '$role', count: { $sum: 1 } } },
  { $sort: { count: -1 } }
]);

// with projection & expressions
await User.aggregate([
  { $match: { age: { $gte: 18 } } },
  {
    $project: {
      name: 1,
      email: 1,
      isAdult: { $gte: ['$age', 18] },
      year: { $year: '$created_at' }
    }
  }
]);
```

</details>

---

## Aggregation Stages (Complete)

```js
$match        // filter documents
$project      // reshape documents
$addFields    // add computed fields
$set          // alias of $addFields
$unset        // remove fields
$group        // aggregation
$sort         // ordering
$limit        // limit docs
$skip         // skip docs
$unwind       // deconstruct arrays
$lookup       // joins
$facet        // multi pipelines
$bucket       // grouping ranges
$bucketAuto   // automatic buckets
$sample       // random sample
$count        // count docs
$replaceRoot  // replace document root
$replaceWith  // alias
$sortByCount  // group + sort
$merge        // write results
$out          // write results (collection replace)
```

<details>
<summary>Examples</summary>

```js
await User.aggregate([
  { $unwind: '$roles' },
  { $sortByCount: '$roles' }
]);

await User.aggregate([
  { $bucket: { groupBy: '$age', boundaries: [18,30,50,100], default: 'other' } }
]);
```

</details>

---

## Expressions & Operators

```js
// arithmetic
$add $subtract $multiply $divide $mod

// comparison
$eq $ne $gt $gte $lt $lte

// boolean
$and $or $not

// conditional
$cond $ifNull $switch

// array
$map $filter $reduce $size $arrayElemAt

// string
$concat $substr $toLower $toUpper $trim

// date
$year $month $dayOfMonth $dateDiff
```

<details>
<summary>Examples</summary>

```js
await User.aggregate([
  {
    $project: {
      emailDomain: { $arrayElemAt: [{ $split: ['$email', '@'] }, 1] },
      tagsCount: { $size: '$tags' }
    }
  }
]);
```

</details>

---

## Aggregation Options & Helpers

```js
agg.allowDiskUse(true)        // boolean
agg.collation(options)       // object
agg.hint(index)               // object | string
agg.cursor({ batchSize })     // stream
agg.read(readPref)            // read preference
agg.explain()                 // execution stats
```

<details>
<summary>Examples</summary>

```js
await User.aggregate([{ $match: { isActive: true } }])
  .allowDiskUse(true)
  .hint({ isActive: 1 })
  .explain('executionStats');
```

</details>

---

## Lookups (Advanced)

```js
// simple
$lookup: { from, localField, foreignField, as }

// pipeline
$lookup: { from, let, pipeline, as }
```

<details>
<summary>Examples</summary>

```js
await User.aggregate([
  {
    $lookup: {
      from: 'organizations',
      let: { orgId: '$organizationId' },
      pipeline: [
        { $match: { $expr: { $eq: ['$_id', '$$orgId'] } } },
        { $project: { name: 1 } }
      ],
      as: 'org'
    }
  }
]);
```

</details>

---

## Facets & Analytics Patterns

```js
$facet
```

<details>
<summary>Examples</summary>

```js
await User.aggregate([
  { $match: { isActive: true } },
  {
    $facet: {
      data: [{ $sort: { created_at: -1 } }, { $limit: 10 }],
      total: [{ $count: 'count' }],
      roles: [{ $sortByCount: '$role' }]
    }
  }
]);
```

</details>

---

## Write Aggregations

```js
$out     // replaces collection
$merge   // merge into collection
```

<details>
<summary>Examples</summary>

```js
await User.aggregate([
  { $match: { isActive: false } },
  { $merge: { into: 'inactive_users', whenMatched: 'replace' } }
]);
```

</details>

---

## Aggregation – Full End‑to‑End Example (Everything Combined)

<details>
<summary>Complete aggregation usage example</summary>

```js
await User.aggregate([
  // 1️⃣ Early filter (uses index if present)
  {
    $match: {
      isActive: true,
      age: { $gte: 18, $lte: 60 },           // comparison
      roles: { $in: ['user', 'admin'] }     // array
    }
  },

  // 2️⃣ Join with another collection (pipeline lookup)
  {
    $lookup: {
      from: 'organizations',
      let: { orgId: '$organizationId' },
      pipeline: [
        {
          $match: {
            $expr: { $eq: ['$_id', '$$orgId'] } // correlated subquery
          }
        },
        { $project: { name: 1, tier: 1 } }     // projection inside lookup
      ],
      as: 'organization'
    }
  },

  // 3️⃣ Normalize joined array
  { $unwind: '$organization' },

  // 4️⃣ Compute derived fields
  {
    $addFields: {
      emailDomain: {                        // string + array ops
        $arrayElemAt: [{ $split: ['$email', '@'] }, 1]
      },
      isAdult: { $gte: ['$age', 18] },      // comparison
      roleCount: { $size: '$roles' }        // array size
    }
  },

  // 5️⃣ Reshape output
  {
    $project: {
      name: 1,
      email: 1,
      emailDomain: 1,
      isAdult: 1,
      roleCount: 1,
      'organization.name': 1,
      'organization.tier': 1,
      createdYear: { $year: '$created_at' } // date operator
    }
  },

  // 6️⃣ Faceted analytics (data + metadata in one query)
  {
    $facet: {
      data: [
        { $sort: { createdYear: -1 } },      // sort
        { $skip: 0 },                        // pagination offset
        { $limit: 10 }                       // pagination limit
      ],
      stats: [
        { $group: { _id: '$organization.tier', count: { $sum: 1 } } }
      ]
    }
  }
])
  // 7️⃣ Aggregation-level options
  .allowDiskUse(true)                        // spill to disk
  .hint({ isActive: 1 })                     // force index
  .collation({ locale: 'en', strength: 2 })  // case-insensitive
  .explain('executionStats');                // execution plan
```

</details>

---

## Aggregation Performance Notes

* `$match` should be first whenever possible
* `$lookup` ≠ `populate` (no middleware, no schema)
* `$facet` is CPU heavy but network-efficient
* `$out` / `$merge` require exclusive write intent

---

## Distinct & MapReduce (Legacy)

```js
Model.distinct(field, filter?)
Model.mapReduce(options)
```

<details>
<summary>Examples</summary>

```js
await User.distinct('role', { isActive: true });

// mapReduce (legacy, avoid in new systems)
await User.mapReduce({
  map() { emit(this.role, 1); },
  reduce(key, values) { return Array.sum(values); }
});
```

</details>

---

## Query Helpers & Reuse

```js
schema.query.active = function () {
  return this.where({ isActive: true });
};
```

<details>
<summary>Examples</summary>

```js
await User.find().active().limit(10);
```

</details>

---

## Notes

* Queries are **lazy** until executed
* Order of chaining matters (`sort` before `limit`)
* Prefer **lean + projection** for read-heavy paths

## Caveats

* Overusing populate causes N+1 queries
* Skip-based pagination degrades on large collections
* Always validate indexes with `explain()`
