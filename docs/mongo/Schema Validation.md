---

id: mongodb-schema-validation
title: Schema Validation
sidebar_position: 15
--------------------

# 15. Schema Validation

> Enforcing document structure and constraints at the database layer

---

## JSON Schema Validation

> Uses JSON Schema to validate documents on insert and update

**Interview points**

* Implemented via `$jsonSchema`
* Validates shape, types, required fields
* Applied per collection

<details>
<summary>Examples (Node.js)</summary>

```js
// Create collection with JSON Schema validation
await db.createCollection('users', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['email', 'age'],
      properties: {
        email: {
          bsonType: 'string',
          description: 'must be a string'
        },
        age: {
          bsonType: 'int', // int | long | double | decimal
          minimum: 18
        },
        tags: {
          bsonType: 'array', // array | object | string | number
          items: { bsonType: 'string' }
        }
      }
    }
  }
});
```

</details>

---

## Validation Levels and Actions

> Controls when validation runs and how failures are handled

**Interview points**

* `validationLevel`: strict | moderate
* `validationAction`: error | warn
* Tunable without downtime

<details>
<summary>Examples (Node.js)</summary>

```js
// Modify validation behavior
await db.command({
  collMod: 'users',
  validationLevel: 'moderate', // strict | moderate
  validationAction: 'warn'     // error | warn
});
```

</details>

---

## Migration Strategies

> Evolving schemas without breaking existing data

**Interview points**

* Relax validation during migration
* Backfill data incrementally
* Tighten rules after cleanup

<details>
<summary>Examples (Node.js)</summary>

```js
// Step 1: Relax validation
await db.command({
  collMod: 'users',
  validationAction: 'warn'
});

// Step 2: Backfill missing fields
await db.collection('users').updateMany(
  { age: { $exists: false } },
  { $set: { age: 18 } }
);

// Step 3: Enforce strict validation
await db.command({
  collMod: 'users',
  validationLevel: 'strict',
  validationAction: 'error'
});
```

</details>

---

## Performance Impact

> Cost of validation during write operations

**Interview points**

* Impacts insert/update latency
* Complex schemas cost more CPU
* No impact on reads

<details>
<summary>Examples (Node.js)</summary>

```js
// Write-heavy workload with validation
await db.collection('events').insertMany(docs, {
  ordered: false // reduces latency despite validation
});
```

</details>

---

## Notes

* Validation complements application-level checks
* Best used for critical invariants

## Caveats

* Not a replacement for full schema migrations
* Overly strict schemas reduce agility
* Validation errors surface at write time
