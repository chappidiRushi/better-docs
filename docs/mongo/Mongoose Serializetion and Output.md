---

id: mongoose-serialization-output
title: Serialization and Output
sidebar_position: 37
--------------------

# Serialization and Output

> Control how Mongoose documents are converted to plain objects and JSON

---

## 1. `toObject()` vs `toJSON()`

> Convert hydrated documents into plain representations

```js
doc.toObject(options?)   // → Object
doc.toJSON(options?)     // → Object (used by JSON.stringify)
```

<details>
<summary>Examples</summary>

```js
const doc = await User.findById(id);

// toObject
const obj = doc.toObject({
  getters: false,        // boolean
  virtuals: false,      // boolean
  depopulate: false,    // boolean
  versionKey: true      // boolean | string
});

// toJSON (implicit)
JSON.stringify(doc);     // calls toJSON internally
```

</details>

---

## 2. Transform Functions

> Mutate output shape during serialization

```js
{ toObject: { transform }, toJSON: { transform } }
```

<details>
<summary>Examples</summary>

```js
const schema = new Schema({}, {
  toJSON: {
    virtuals: true,
    transform(doc, ret, options) {
      delete ret._id;             // any mutation
      ret.id = doc._id.toString();
      return ret;                 // object | undefined
    }
  },
  toObject: {
    transform(doc, ret) {
      ret.safe = true;
    }
  }
});
```

</details>

---

## 3. Hiding Fields

> Exclude sensitive or internal fields from output

```js
select: false | transform | delete
```

<details>
<summary>Examples</summary>

```js
// schema-level
const schema = new Schema({
  password: { type: String, select: false }, // boolean
  token: String
});

// transform-based
schema.set('toJSON', {
  transform(doc, ret) {
    delete ret.token;             // manual removal
  }
});

// query-level override
await User.findOne({ email }).select('+password'); // include explicitly
```

</details>

---

## 4. Virtuals in Output

> Computed fields not persisted in MongoDB

```js
schema.virtual(name).get(fn)
```

<details>
<summary>Examples</summary>

```js
const schema = new Schema({
  firstName: String,
  lastName: String
}, {
  toJSON: { virtuals: true },     // boolean
  toObject: { virtuals: true }    // boolean
});

schema.virtual('fullName').get(function () {
  return `${this.firstName} ${this.lastName}`;
});

const user = await User.findOne();
user.toJSON();                    // includes fullName
```

</details>

---

## Notes

* `toJSON` is used implicitly by `res.json` and `JSON.stringify`
* Transforms run after getters and virtuals
* Virtuals never persist to the database

## Caveats

* Lean queries bypass document serialization
* Transform functions run per document (costly in large arrays)
* Hidden fields can still be queried explicitly
