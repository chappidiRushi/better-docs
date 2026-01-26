---

id: mongoose-migrations-schema-evolution
title: Mongoose Migrations and Schema Evolution
sidebar_position: 42
--------------------

# Mongoose Migrations and Schema Evolution

> Evolving schemas safely over time: backward compatibility, data migrations, versioned documents, and deployment strategies

---

## **1. Backward Compatibility**

> Ensure new schema versions can read old documents

```js
optional fields | defaults | tolerant readers
```

```js
// additive change (safe)
const schema = new Schema({
  email: String,
  role: { type: String, default: 'user' } // default for old docs
});

// tolerant read
function normalize(doc) {
  return {
    role: doc.role ?? 'user',     // nullish coalescing
    email: doc.email
  };
}

// avoid breaking rename
// ❌ removing fields before migration
// ✅ read old + new during transition
```

---

## 2. Data Migrations

> One-time transformations of existing data

```js
scripts | batch updates | cursors
```

```js
// cursor-based migration
const cursor = User.find({ role: { $exists: false } }).cursor({
  batchSize: 500               // number
});

for await (const doc of cursor) {
  await User.updateOne(
    { _id: doc._id },
    { $set: { role: 'user' } } // migration logic
  );
}

// bulk migration
await User.updateMany(
  { role: { $exists: false } },
  { $set: { role: 'user' } }
);
```

---

## 3. Versioned Documents

> Track document shape explicitly inside the document

```js
schemaVersion | __v (custom)
```

```js
const schema = new Schema({
  schemaVersion: {
    type: Number,
    required: true,
    default: 1              // number
  },
  email: String
});

function upgrade(doc) {
  if (doc.schemaVersion === 1) {
    doc.schemaVersion = 2;
    doc.role = 'user';
  }
  return doc;
}

// lazy migration on read
const doc = await User.findById(id);
upgrade(doc);
```

---

## 4. Deployment Strategies

> Rollout schema and data changes without downtime

```js
expand → migrate → contract
```

```js
// 1. expand
// deploy code that supports old + new fields

// 2. migrate
await User.updateMany(
  { schemaVersion: 1 },
  { $set: { schemaVersion: 2 } }
);

// 3. contract
// remove legacy fields + code paths in later deploy

// index changes via separate migration
await User.collection.createIndex(
  { email: 1 },
  { unique: true, background: true }
);
```

---

## Notes

* Prefer additive changes over breaking ones
* Data migrations should be idempotent
* Lazy migrations reduce operational risk

## Caveats

* Large migrations must be throttled
* Rollbacks are hard without versioned data
* Schema changes and index changes are separate concerns
