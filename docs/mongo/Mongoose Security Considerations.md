---

id: mongoose-security-considerations
title: Security Considerations
sidebar_position: 40
--------------------

# Mongoose Security Considerations

> Injection surfaces, write controls, schema trust boundaries, and secure-by-default patterns

---

## 1. Injection Risks

> MongoDB operator injection via untrusted input

```js
$ne | $gt | $where | $regex
```

<details>
<summary>Examples</summary>

```js
// ❌ vulnerable
await User.find({ email: req.body.email }); // user controls object shape

// payload
{ "email": { "$ne": null } }

// ✅ safe
await User.find({ email: String(req.body.email) });

// strict filtering
await User.find({
  email: { $eq: req.body.email } // explicit operator
});
```

</details>

---

## 2. Mass Assignment

> Uncontrolled writes via spread or direct body usage

```js
Model.create(input)
Model.updateOne(filter, input)
```

<details>
<summary>Examples</summary>

```js
// ❌ vulnerable
await User.create(req.body);      // role, isAdmin injectable

// ✅ allowlist
const payload = {
  email: req.body.email,
  name: req.body.name
};

await User.create(payload);

// schema-level protection
new Schema({
  role: { type: String, immutable: true }, // boolean
  isAdmin: { type: Boolean, select: false }
});
```

</details>

---

## 3. Schema Enforcement Limits

> Schema validation is not a security boundary

```js
strict | runValidators | overwrite
```

<details>
<summary>Examples</summary>

```js
// strict mode
new Schema({}, {
  strict: true               // true | false | 'throw'
});

// update bypass
await User.updateOne(
  { _id: id },
  { role: 'admin' },         // bypasses unless validated
  { runValidators: true }    // boolean
);

// overwrite risk
await User.replaceOne(
  { _id: id },
  req.body                   // full replacement
);
```

</details>

---

## 4. Secure Defaults

> Baseline configuration for production safety

```js
autoIndex | strictQuery | sanitizeFilter
```

<details>
<summary>Examples</summary>

```js
// global defaults
mongoose.set('autoIndex', false);        // boolean
mongoose.set('strictQuery', true);       // boolean
mongoose.set('sanitizeFilter', true);    // boolean

// schema defaults
new Schema({}, {
  strict: 'throw',            // reject unknown fields
  minimize: true              // remove empty objects
});

// query hardening
await User.find(req.query)
  .select('email name');      // projection allowlist
```

</details>

---

## Notes

* Mongoose does not sanitize input by default
* Schemas protect structure, not intent
* Most vulnerabilities originate before Mongoose

## Caveats

* `$where` enables JS execution — never expose
* Validation does not prevent logical privilege escalation
* Security requires API + DB + schema alignment
