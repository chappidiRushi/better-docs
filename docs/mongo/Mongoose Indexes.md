---

id: mongoose-indexes
title: Mongoose Indexes
sidebar_position: 33
--------------------

# Mongoose Indexes

> Schema-level index definitions, sync behavior, build strategies, and production-safe practices

---

## 1. Schema Index Definitions

> Define indexes at schema level using `index`, `schema.index()`, or compound definitions

```js
// Single-field, compound, unique, sparse, partial, TTL
```

<details>
<summary>Examples</summary>

```js
const UserSchema = new Schema({
  email: {
    type: String,
    index: true,              // boolean | object
    unique: true,             // boolean
    sparse: true              // boolean
  },
  age: Number,
  status: String,
  createdAt: Date
}, {
  timestamps: true
});

// Compound index
UserSchema.index(
  { email: 1, status: 1 },     // 1 | -1 | 'text' | 'hashed'
  { unique: true, background: true }
);

// Partial index
UserSchema.index(
  { age: 1 },
  { partialFilterExpression: { age: { $gte: 18 } } }
);

// TTL index
UserSchema.index(
  { createdAt: 1 },
  { expireAfterSeconds: 3600 } // seconds | 0
);
```

</details>

---

## 2. Index Synchronization

> Sync schema-defined indexes with MongoDB using `autoIndex`, `syncIndexes`, or `createIndexes`

```js
Model.syncIndexes(options?)     // → Promise<object>
Model.createIndexes(options?)  // → Promise<void>
```

<details>
<summary>Examples</summary>

```js
// Global
mongoose.set('autoIndex', false); // boolean

// Per schema
new Schema({}, { autoIndex: false }); // boolean

// Runtime sync
await User.syncIndexes({
  background: true,             // boolean
  continueOnError: true         // boolean
});

// Explicit creation (no drop)
await User.createIndexes({
  background: true              // boolean
});
```

</details>

---

## 3. Background Builds

> Control index build behavior to avoid blocking writes

```js
{ background: true }             // MongoDB < 4.2 relevant
```

<details>
<summary>Examples</summary>

```js
// Schema-level
UserSchema.index(
  { email: 1 },
  { background: true }          // boolean
);

// syncIndexes
await User.syncIndexes({
  background: true              // boolean
});

// Note: MongoDB 4.2+ builds indexes in foreground by default without blocking
```

</details>

---

## 4. Production Index Strategy

> Safe index management patterns for production systems

```js
// Disable autoIndex, manage via migrations
```

<details>
<summary>Examples</summary>

```js
// app bootstrap
mongoose.set('autoIndex', false); // ALWAYS false in prod

// CI / migration script
await User.syncIndexes({
  background: true,
  continueOnError: false
});

// Inspect
await User.collection.getIndexes(); // existing indexes

// Drop manually if needed
await User.collection.dropIndex('email_1'); // string | object
```

</details>

---

## Notes

* Schema indexes are declarative, DB indexes are imperative
* `syncIndexes` DROPS extra indexes not in schema
* `createIndexes` only creates missing ones
* Partial + compound indexes outperform sparse in most cases

## Caveats

* Never enable `autoIndex` in production
* Index order matters in compound indexes
* TTL works only on `Date` fields
* Text indexes are limited to one per collection
