---

id: mongoose-models-compilation
title: Mongoose Models and Compilation
sidebar_position: 26
--------------------

# 26. Mongoose Models and Compilation

> How schemas are compiled into models and how Mongoose manages model identity

---

## Model Compilation

> Converting a schema into an executable model bound to a collection

**Interview points**

* Happens once per connection
* Creates query + document constructors
* Compilation is synchronous

<details>
<summary>Examples (Node.js)</summary>

```js
import mongoose from 'mongoose';

const schema = new mongoose.Schema({ name: String });

// Compilation
const User = mongoose.model('User', schema);
```

</details>

---

## Model Caching

> Internal registry preventing duplicate model compilation

**Interview points**

* Cached per connection
* Re-compiling same name throws error
* Common issue in hot-reload environments

<details>
<summary>Examples (Node.js)</summary>

```js
// Throws OverwriteModelError if already compiled
mongoose.model('User', schema);

// Safe retrieval
const User = mongoose.models.User || mongoose.model('User', schema);
```

</details>

---

## Discriminators

> Model inheritance using a shared collection

**Interview points**

* Single collection, multiple schemas
* Uses discriminatorKey
* Avoids polymorphic collections

<details>
<summary>Examples (Node.js)</summary>

```js
const options = { discriminatorKey: 'kind' };

const baseSchema = new mongoose.Schema({ name: String }, options);
const Base = mongoose.model('Base', baseSchema);

const Child = Base.discriminator('Child', new mongoose.Schema({ age: Number }));
```

</details>

---

## Multi-Connection Models

> Same schema compiled against different connections

**Interview points**

* Models are connection-scoped
* Required for multi-DB or multi-tenant systems
* No shared state between models

<details>
<summary>Examples (Node.js)</summary>

```js
const connA = mongoose.createConnection('mongodb://localhost/dbA');
const connB = mongoose.createConnection('mongodb://localhost/dbB');

const UserA = connA.model('User', schema);
const UserB = connB.model('User', schema);
```

</details>

---

## Collection Name Resolution

> How Mongoose maps models to MongoDB collections

**Interview points**

* Pluralizes model name by default
* Can be overridden explicitly
* Case-sensitive at MongoDB level

<details>
<summary>Examples (Node.js)</summary>

```js
// Default: 'users'
mongoose.model('User', schema);

// Explicit collection name
mongoose.model('User', schema, 'app_users');
```

</details>

---

## Notes

* Model identity = connection + name
* Discriminators are schema-level abstraction

## Caveats

* Hot reload causes model overwrite errors
* Discriminators complicate indexing
* Wrong collection names lead to silent bugs
