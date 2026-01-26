---

id: mongoose-error-handling
title: Error Handling
sidebar_position: 38
--------------------

# Error Handling

> Mongoose error classes, MongoDB server errors, and normalization patterns

---

## 1. Error Hierarchy

> All Mongoose errors extend `MongooseError`

```js
MongooseError
 ├─ CastError
 ├─ ValidationError
 ├─ VersionError
 ├─ DocumentNotFoundError
 └─ MongoServerError (wrapped)
```

<details>
<summary>Examples</summary>

```js
try {
  await User.findById('invalid');
} catch (err) {
  err instanceof mongoose.Error;              // true
  err.name;                                   // string
  err.message;                                // string
}
```

</details>

---

## 2. Cast Errors

> Thrown when value cannot be cast to schema type

```js
CastError { path, value, kind, reason }
```

<details>
<summary>Examples</summary>

```js
try {
  await User.findById('abc');                  // invalid ObjectId
} catch (err) {
  err.name === 'CastError';                    // true
  err.path;                                   // '_id'
  err.kind;                                   // 'ObjectId'
  err.value;                                  // 'abc'
}
```

</details>

---

## 3. Validation Errors

> Aggregated errors from schema validation failures

```js
ValidationError { errors: { [path]: ValidatorError } }
```

<details>
<summary>Examples</summary>

```js
try {
  await User.create({ email: 'not-an-email' });
} catch (err) {
  err.name === 'ValidationError';              // true
  Object.keys(err.errors);                     // ['email']

  err.errors.email.kind;                       // 'regexp' | 'required' | 'min'
  err.errors.email.message;                    // string
}
```

</details>

---

## 4. MongoDB Server Errors

> Errors returned directly from MongoDB server

```js
MongoServerError { code, keyPattern, keyValue }
```

<details>
<summary>Examples</summary>

```js
try {
  await User.create({ email: 'a@test.com' });
  await User.create({ email: 'a@test.com' }); // duplicate
} catch (err) {
  err.name === 'MongoServerError';             // true
  err.code === 11000;                          // duplicate key
  err.keyPattern;                             // { email: 1 }
  err.keyValue;                               // { email: 'a@test.com' }
}
```

</details>

---

## 5. Error Normalization

> Convert heterogeneous errors into consistent application-level format

```js
normalize(err) → { type, message, details }
```

<details>
<summary>Examples</summary>

```js
function normalizeError(err) {
  if (err instanceof mongoose.Error.CastError) {
    return { type: 'CAST', message: 'Invalid ID', details: err.path };
  }

  if (err instanceof mongoose.Error.ValidationError) {
    return {
      type: 'VALIDATION',
      message: 'Validation failed',
      details: Object.values(err.errors).map(e => e.message)
    };
  }

  if (err.code === 11000) {
    return {
      type: 'DUPLICATE',
      message: 'Duplicate key',
      details: err.keyValue
    };
  }

  return { type: 'UNKNOWN', message: err.message };
}
```

</details>

---

## Notes

* Mongoose wraps MongoDB driver errors
* Validation errors can contain multiple field failures
* Error `name` is more stable than message

## Caveats

* Lean queries still throw MongoDB errors
* `updateOne` validation errors require `runValidators`
* Network errors bypass Mongoose abstractions
