---

id: mongoose-validation
title: Mongoose Validation System
sidebar_position: 28
--------------------

# 28. Mongoose Validation System

> Client-side schema validation layer enforced before MongoDB writes

---

## Validation Lifecycle

> When and how validation runs in Mongoose

**Interview points**

* Runs before `save()`, `create()`, and certain updates
* Occurs **before** data is sent to MongoDB
* Can be triggered manually

<details>
<summary>Examples (Node.js)</summary>

```js
const user = new User({ age: -1 });

await user.validate(); // throws ValidationError
await user.save();     // also triggers validation
```

</details>

---

## Built-in Validators

> Schema-level validation provided by Mongoose

**Interview points**

* Declarative and synchronous
* Applied per SchemaType
* Enforced only in Mongoose, not MongoDB

**Common validators**

* `required`
* `min` / `max`
* `enum`
* `match`
* `minlength` / `maxlength`

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({
  age: { type: Number, min: 0, max: 120, required: true },
  role: { type: String, enum: ['user', 'admin'] }
});
```

</details>

---

## Custom and Async Validators

> User-defined validation logic

**Interview points**

* Can be synchronous or asynchronous
* Async validators must return a promise
* Useful for cross-field or DB checks

<details>
<summary>Examples (Node.js)</summary>

```js
const schema = new mongoose.Schema({
  email: {
    type: String,
    validate: {
      validator: async function (v) {
        return !(await User.exists({ email: v }));
      },
      message: 'Email already exists'
    }
  }
});
```

</details>

---

## Validation Errors

> Error structure produced by failed validation

**Interview points**

* Error type: `ValidationError`
* Contains per-path error details
* Safe to expose to clients

<details>
<summary>Examples (Node.js)</summary>

```js
try {
  await user.save();
} catch (err) {
  err.name; // 'ValidationError'
  err.errors.age.message;
}
```

</details>

---

## Skipping Validation

> Bypassing validation for performance or migrations

**Interview points**

* Possible but dangerous
* Often used in bulk or legacy writes
* Does NOT bypass MongoDB schema validation

<details>
<summary>Examples (Node.js)</summary>

```js
await user.save({ validateBeforeSave: false });

await User.updateOne(
  { _id },
  { $set: { age: -5 } },
  { runValidators: false }
);
```

</details>

---

## Notes

* Validation happens **client-side only**
* MongoDB itself does not enforce Mongoose rules

## Caveats

* Update queries skip validation by default
* Async validators impact write latency
* Validation is not a substitute for DB-level constraints
