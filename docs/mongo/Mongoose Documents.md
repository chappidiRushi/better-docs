---

id: mongoose-documents
title: Mongoose Documents
sidebar_position: 27
--------------------

# Mongoose Documents

> Runtime representations of MongoDB documents with state, behavior, and lifecycle

---

## Document Instantiation

> Creating document instances before persistence

**Interview points**

* Instantiated via model constructor
* Exists only in memory until saved
* Defaults applied at creation time

<details>
<summary>Examples (Node.js)</summary>

```js
const user = new User({ name: 'Alice', age: 30 });

user.isNew; // true
```

</details>

---

## Change Tracking

> Internal bookkeeping of modified paths

**Interview points**

* Tracks field-level mutations
* Enables minimal updates
* Reset after successful save

<details>
<summary>Examples (Node.js)</summary>

```js
user.name = 'Bob';

user.modifiedPaths(); // ['name']
```

</details>

---

## Dirty Checking

> Detecting unsaved changes in documents

**Interview points**

* Performed client-side
* Used to generate update operators
* Can be manually overridden

<details>
<summary>Examples (Node.js)</summary>

```js
user.isModified('name'); // true | false

user.markModified('meta'); // for Mixed types
```

</details>

---

## Virtuals

> Computed properties not persisted to MongoDB

**Interview points**

* Derived fields
* Included optionally in JSON/Object output
* Zero storage cost

<details>
<summary>Examples (Node.js)</summary>

```js
userSchema.virtual('isAdult').get(function () {
  return this.age >= 18;
});

user.isAdult; // computed
```

</details>

---

## Getters and Setters

> Transforming values on read and write

**Interview points**

* Applied during casting and retrieval
* Does not affect stored value directly
* Can hide data inconsistencies

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({
  email: {
    type: String,
    set: v => v.toLowerCase(), // setter
    get: v => v.toUpperCase(), // getter
  }
});
```

</details>

---

## Lean Documents

> Skipping document hydration for performance

**Interview points**

* Returns plain JS objects
* No getters, setters, or methods
* Preferred for read-heavy paths

<details>
<summary>Examples (Node.js)</summary>

```js
await User.findOne({ name: 'Alice' }).lean(); // plain object
```

</details>

---

## Hydration

> Converting raw MongoDB results into Mongoose documents

**Interview points**

* Automatic for normal queries
* Expensive at scale
* Avoidable via lean

<details>
<summary>Examples (Node.js)</summary>

```js
// Manual hydration
const raw = await db.collection('users').findOne({});
const doc = User.hydrate(raw);
```

</details>

---

## Notes

* Documents encapsulate state + behavior
* Hydration is the main performance cost

## Caveats

* Overusing documents hurts throughput
* Lean queries bypass validation and hooks
* Virtuals can mask missing data
