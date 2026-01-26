---

id: mongoose-delete-operations
title: Delete Operations
sidebar_position: 36
--------------------

# Delete Operations

> Hard deletes, middleware behavior, soft-delete patterns, and cascading strategies

---

## 1. Delete Methods

> Atomic delete APIs at model and document level

```js
Model.deleteOne(filter, options?)        // → Promise<DeleteResult>
Model.deleteMany(filter, options?)       // → Promise<DeleteResult>
Model.findOneAndDelete(filter, options?) // → Promise<Document|null>
Document.deleteOne(options?)             // → Promise<DeleteResult>
```

<details>
<summary>Examples</summary>

```js
// deleteOne
await User.deleteOne(
  { _id: id },                 // filter: object
  { session }                  // options: session | collation
);

// deleteMany
await Session.deleteMany(
  { expired: true },           // filter
  { session }                  // options
);

// findOneAndDelete
await User.findOneAndDelete(
  { email },
  {
    projection: { email: 1 },  // object | null
    session                    // ClientSession
  }
);
```

</details>

---

## 2. Middleware Interaction

> Delete middleware differs by method and level

```js
pre('deleteOne')
pre('deleteMany')
pre('findOneAndDelete')
```

<details>
<summary>Examples</summary>

```js
// Query middleware
schema.pre('deleteOne', { document: false, query: true }, async function () {
  // this = query
});

schema.pre('findOneAndDelete', async function () {
  // this = query
});

// Document middleware
schema.pre('deleteOne', { document: true }, async function () {
  // this = document
});
```

</details>

---

## 3. Soft Deletes

> Logical deletion using flags instead of physical removal

```js
{ deletedAt, isDeleted }
```

<details>
<summary>Examples</summary>

```js
// schema
const schema = new Schema({
  deletedAt: Date,
  isDeleted: { type: Boolean, default: false }
});

// soft delete
await User.updateOne(
  { _id: id },
  {
    $set: {
      isDeleted: true,
      deletedAt: new Date()
    }
  }
);

// global query filter
schema.pre(/^find/, function () {
  this.where({ isDeleted: false });
});
```

</details>

---

## 4. Cascading Deletes

> Manual or middleware-driven cleanup of dependent documents

```js
// Application-level cascade
```

<details>
<summary>Examples</summary>

```js
// pre delete cascade
schema.pre('findOneAndDelete', async function () {
  const doc = await this.model.findOne(this.getQuery());

  await Order.deleteMany({
    userId: doc._id               // foreign key
  });
});

// transactional cascade
const session = await mongoose.startSession();

await session.withTransaction(async () => {
  await User.deleteOne({ _id: id }, { session });
  await Order.deleteMany({ userId: id }, { session });
});

session.endSession();
```

</details>

---

## Notes

* Deletes are atomic per document
* `findOneAndDelete` returns the deleted document
* MongoDB has no native cascade delete

## Caveats

* `deleteMany` bypasses document middleware
* Soft deletes must be enforced at query level
* Cascades should use transactions when consistency matters
