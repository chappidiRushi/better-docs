---

id: mongoose-middleware
title: Mongoose Middleware (Hooks)
sidebar_position: 29
--------------------

# 29. Mongoose Middleware (Hooks)

> Interception layer around document, query, and model operations

---

## Middleware Types

> Classification based on what operation is being intercepted

**Interview points**

* Document middleware → `save`, `validate`, `remove`
* Query middleware → `find`, `update`, `delete`
* Model middleware → `insertMany`
* Aggregate middleware → `aggregate`

<details>
<summary>Examples (Node.js)</summary>

```js
// Document middleware
schema.pre('save', function () {
  this.updatedAt = new Date();
});

// Query middleware
schema.pre('findOne', function () {
  this.where({ deleted: false });
});

// Model middleware
schema.pre('insertMany', function (next, docs) {
  docs.forEach(d => d.createdAt = new Date());
  next();
});

// Aggregate middleware
schema.pre('aggregate', function () {
  this.pipeline().unshift({ $match: { active: true } });
});
```

</details>

---

## Pre vs Post Hooks

> Hooks executed before or after the core operation

**Interview points**

* `pre` can mutate data or abort execution
* `post` observes results, cannot stop execution
* `pre` supports async / `next()`

<details>
<summary>Examples (Node.js)</summary>

```js
schema.pre('save', async function () {
  this.password = await hash(this.password);
});

schema.post('save', function (doc) {
  auditLog(doc._id, 'created');
});
```

</details>

---

## Execution Order

> Deterministic ordering of multiple middleware functions

**Interview points**

* Executes in registration order
* `pre` hooks run top-down
* `post` hooks run bottom-up

<details>
<summary>Examples (Node.js)</summary>

```js
schema.pre('save', function () {
  console.log('pre-1');
});

schema.pre('save', function () {
  console.log('pre-2');
});

schema.post('save', function () {
  console.log('post-2');
});

schema.post('save', function () {
  console.log('post-1');
});

// Output order:
// pre-1 → pre-2 → post-2 → post-1
```

</details>

---

## Error Handling

> Propagating failures from middleware

**Interview points**

* Throwing or rejecting aborts the operation
* Errors bubble to the caller
* Post hooks receive error as first arg

<details>
<summary>Examples (Node.js)</summary>

```js
schema.pre('save', function () {
  if (!this.isValid()) {
    throw new Error('Invalid state');
  }
});

schema.post('save', function (error, doc, next) {
  if (error) {
    console.error('Save failed', error);
  }
  next(error);
});
```

</details>

---

## Performance Pitfalls

> Common middleware-related anti-patterns

**Interview points**

* Heavy logic in `pre('save')` blocks writes
* Query middleware can break index usage
* Async hooks add latency
* Hidden side effects reduce debuggability

<details>
<summary>Examples (Node.js)</summary>

```js
// ❌ Expensive DB call per document save
schema.pre('save', async function () {
  this.counter = await Counter.increment();
});

// ❌ Query middleware modifying selectors
schema.pre('find', function () {
  this.where({ status: { $ne: 'archived' } }); // may bypass index
});

// ✅ Prefer explicit service-layer logic
```

</details>

---

## Notes

* Middleware runs **only in Mongoose**, not MongoDB
* `lean()` queries bypass document middleware
* Update queries use **query middleware**, not document

## Caveats

* `save()` middleware does NOT run on `updateOne`
* `insertMany()` middleware is opt-in
* Debugging nested hooks is non-trivial
