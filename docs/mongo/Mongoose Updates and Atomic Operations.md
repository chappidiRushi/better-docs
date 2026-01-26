---

id: mongoose-updates-atomic
title: Mongoose Updates and Atomic Operations
sidebar_position: 34
--------------------

# Mongoose Updates and Atomic Operations

> Atomic update methods, versioning, concurrency control, array mutations, and upsert patterns

---

## 1. Update Methods

> Atomic update APIs that bypass document hydration unless explicitly requested

```js
Model.updateOne(filter, update, options?)      // → Promise<UpdateResult>
Model.updateMany(filter, update, options?)     // → Promise<UpdateResult>
Model.findOneAndUpdate(filter, update, options?) // → Promise<Document|null>
Document.updateOne(update, options?)           // → Promise<UpdateResult>
```

<details>
<summary>Examples</summary>

```js
// updateOne
await User.updateOne(
  { _id: id },                    // filter: object
  { $set: { name: 'A' } },        // update: operators | replacement
  { upsert: false }               // options: upsert | runValidators | strict
);

// updateMany
await User.updateMany(
  { status: 'inactive' },         // filter
  { $unset: { token: 1 } },       // update
  { multi: true }                 // multi ignored (legacy)
);

// findOneAndUpdate
await User.findOneAndUpdate(
  { email },
  { $inc: { loginCount: 1 } },
  {
    new: true,                    // boolean (return updated)
    upsert: false,                // boolean
    runValidators: true,          // boolean
    setDefaultsOnInsert: true     // boolean
  }
);
```

</details>

---

## 2. Optimistic Concurrency

> Detect write conflicts using version key comparison

```js
{ optimisticConcurrency: true }   // schema option
```

<details>
<summary>Examples</summary>

```js
const schema = new Schema(
  { name: String },
  {
    optimisticConcurrency: true,  // boolean
    versionKey: '__v'              // string | false
  }
);

const doc = await Model.findById(id);
doc.name = 'A';
await doc.save();                  // throws VersionError on conflict
```

</details>

---

## 3. Versioning (`__v`)

> Internal version key used for concurrency and conflict detection

```js
versionKey: '__v' | false | string
```

<details>
<summary>Examples</summary>

```js
// Default behavior
await Model.updateOne(
  { _id: id, __v: 3 },             // version match
  { $set: { name: 'B' }, $inc: { __v: 1 } }
);

// Disable versioning
new Schema({}, { versionKey: false }); // false

// Custom version key
new Schema({}, { versionKey: 'v' });   // string
```

</details>

---

## 4. Array Updates

> Atomic mutations on array fields without full document replacement

```js
$push | $pull | $addToSet | $pop | $pullAll | $each | $position
```

<details>
<summary>Examples</summary>

```js
await User.updateOne(
  { _id: id },
  {
    $push: {
      tags: {
        $each: ['a', 'b'],         // array
        $position: 0,              // number
        $slice: 5                  // number
      }
    },
    $pull: { roles: 'admin' },    // value | condition
    $addToSet: { skills: 'js' }   // value | { $each: [] }
  }
);
```

</details>

---

## 5. Upserts

> Insert-if-not-exists semantics combined with atomic updates

```js
{ upsert: true }
```

<details>
<summary>Examples</summary>

```js
await User.findOneAndUpdate(
  { email },
  {
    $set: { lastLogin: new Date() },
    $setOnInsert: {
      email,
      role: 'user'                 // any value
    }
  },
  {
    upsert: true,                  // boolean
    new: true,                     // boolean
    rawResult: false               // boolean
  }
);
```

</details>

---

## Notes

* Update methods are atomic at document level
* `findOneAndUpdate` triggers middleware differently than `save`
* `$setOnInsert` only applies on insert

## Caveats

* Update queries bypass schema setters unless `runValidators` is enabled
* Versioning does not protect `updateMany`
* Array positional operators require matching query conditions
