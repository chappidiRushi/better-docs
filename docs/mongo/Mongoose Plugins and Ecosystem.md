---

id: mongoose-plugins-ecosystem
title: Mongoose Plugins and Ecosystem
sidebar_position: 43
--------------------

# Mongoose Plugins and Ecosystem

> Cross-cutting schema extensions, plugin authoring patterns, performance risks, and ecosystem trade-offs

---

## 1. Plugin Architecture

> Plugins are functions applied to schemas to extend behavior

```js
schema.plugin(fn, options?)
```

<details>
<summary>Examples</summary>

```js
function plugin(schema, options) {
  // schema = Schema instance
  // options = any
}

const schema = new Schema({ email: String });

schema.plugin(plugin, {
  enabled: true,            // any value
  paths: ['email']          // any value
});

// global plugin
mongoose.plugin(plugin, { global: true });
```

</details>

---

## 2. Writing Plugins

> Encapsulate fields, middleware, statics, methods, and indexes

```js
schema.add | schema.pre | schema.methods | schema.statics
```

<details>
<summary>Examples</summary>

```js
function timestampsPlugin(schema, options) {
  schema.add({
    createdAt: Date,
    updatedAt: Date
  });

  schema.pre('save', function () {
    this.updatedAt = new Date();
    if (!this.createdAt) this.createdAt = this.updatedAt;
  });

  schema.methods.touch = function () {
    this.updatedAt = new Date();
    return this.save();
  };

  schema.statics.findRecent = function () {
    return this.find().sort({ createdAt: -1 });
  };

  if (options.index) {
    schema.index({ createdAt: 1 }); // 1 | -1
  }
}
```

</details>

---

## 3. Plugin Performance Risks

> Plugins can silently degrade performance or alter semantics

```js
middleware | hydration | query hooks
```

<details>
<summary>Examples</summary>

```js
// hidden cost: pre find hook
schema.pre(/^find/, function () {
  this.where({ isDeleted: false }); // runs on EVERY query
});

// hydration overhead
schema.plugin(require('mongoose-autopopulate'));

// index explosion risk
schema.plugin((schema) => {
  schema.index({ field: 1 });
});
```

</details>

---

## 4. Popular Plugins Analysis

> Trade-offs of commonly used community plugins

```js
autopopulate | paginate | soft-delete | lean-virtuals
```

<details>
<summary>Examples</summary>

```js
// mongoose-autopopulate
schema.plugin(require('mongoose-autopopulate'));
// ✔ convenience
// ✘ hidden queries, poor scalability

// mongoose-paginate-v2
schema.plugin(require('mongoose-paginate-v2'));
// ✔ standard pagination
// ✘ abstraction over skip/limit costs

// soft delete plugins
schema.plugin(require('mongoose-delete'), {
  deletedAt: true,          // boolean
  overrideMethods: true    // boolean
});
// ✔ consistent API
// ✘ query magic, index misuse

// lean virtuals
schema.plugin(require('mongoose-lean-virtuals'));
// ✔ restore virtuals with lean
// ✘ extra CPU per doc
```

</details>

---

## Notes

* Plugins run at schema compilation time
* Order of plugin application matters
* Prefer local plugins over global ones

## Caveats

* Global plugins affect every schema implicitly
* Plugins can break lean/query assumptions
* Audit plugin source before production use
