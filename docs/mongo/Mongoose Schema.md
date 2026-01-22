---

id: mongoose-schemas-schematypes
title: Mongoose Schemas and SchemaTypes
sidebar_position: 25
--------------------

# 25. Mongoose Schemas and SchemaTypes

> Defining structure, types, constraints, and behavior of documents in Mongoose

---

## Schema Construction

> Declarative blueprint for documents enforced at write-time

**Interview points**

* Schema-on-write abstraction
* Controls casting, validation, defaults
* Independent of MongoDB server

<details>
<summary>Examples (Node.js)</summary>

```js
import mongoose from 'mongoose';

const userSchema = new mongoose.Schema({
  name: String,                 // shorthand
  age: { type: Number },        // explicit
  email: { type: String, index: true },
}, {
  timestamps: true,             // boolean | object
  versionKey: '__v',             // string | false
});
```

</details>

---

## Built-in SchemaTypes

> Native data types supported by Mongoose

**Interview points**

* Extend MongoDB BSON types
* Handle casting automatically
* Type mismatch throws at runtime

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({
  str: String,                  // String
  num: Number,                  // Number
  bool: Boolean,                // Boolean
  date: Date,                   // Date
  buf: Buffer,                  // Buffer
  id: mongoose.Schema.Types.ObjectId, // ObjectId
  arr: [String],                // Array
  map: { type: Map, of: Number },// Map
  mixed: mongoose.Schema.Types.Mixed, // Mixed
});
```

</details>

---

## Custom SchemaTypes

> Extending Mongoose with domain-specific types

**Interview points**

* Built on top of SchemaType
* Rarely needed
* Increases complexity

<details>
<summary>Examples (Node.js)</summary>

```js
class LowercaseString extends mongoose.SchemaType {
  cast(val) {
    if (typeof val !== 'string') throw new Error('String required');
    return val.toLowerCase();
  }
}

mongoose.Schema.Types.LowercaseString = LowercaseString;

const schema = new mongoose.Schema({
  email: { type: LowercaseString },
});
```

</details>

---

## Default Values

> Automatically assigned values on document creation

**Interview points**

* Applied at instantiation time
* Can be static or function
* Not applied on updates by default

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({
  role: { type: String, default: 'user' },        // static
  createdAt: { type: Date, default: Date.now },  // function
});
```

</details>

---

## Immutability

> Preventing changes to fields after creation

**Interview points**

* Enforced by Mongoose
* Does not affect MongoDB directly
* Useful for IDs and audit fields

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({
  email: { type: String, immutable: true }, // boolean | function
});
```

</details>

---

## Strict Mode Variants

> Handling of unknown fields

**Interview points**

* Applied at schema level
* Affects writes only
* Helps catch data drift

<details>
<summary>Examples (Node.js)</summary>

```js
new mongoose.Schema({ name: String }, {
  strict: true,    // true | false | 'throw'
});
```

</details>

---

## Schema Options

> Global behavior configuration for schemas

**Interview points**

* Affects validation, serialization, and writes
* Can be overridden per schema
* Important for performance tuning

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({ name: String }, {
  strict: true,
  timestamps: true,
  minimize: true,      // remove empty objects
  autoIndex: false,    // disable index build in prod
  toJSON: { virtuals: true },
  toObject: { virtuals: true },
});
```

</details>

---

## Notes

* SchemaTypes control casting and validation
* Defaults and immutability are client-side guarantees

## Caveats

* Mongoose schemas do not enforce server-side constraints
* Mixed types disable validation
* Overuse of custom types complicates maintenance
