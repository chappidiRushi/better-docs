---

id: mongodb-bson-storage
title: BSON, Data Types, and Storage Model
sidebar_position: 3
-------------------

# 2. BSON, Data Types, and Storage Model

> Low-level representation, type system, ordering, and physical constraints

---

## BSON Encoding

> Binary JSON optimized for speed, traversal, and type fidelity

**Characteristics**

* Little-endian binary format
* Type-prefixed fields
* Length-prefixed documents
* Efficient in-place traversal

<details>
<summary>Examples</summary>

```js
// BSON structure (conceptual)
{
  <int32 totalSize>,
  <byte type>, <cstring key>, <value>,
  ...,
  <0x00>
}

// Observe BSON size impact
Object.bsonsize({ a: 1, b: 'x', c: [1, 2, 3] }); // returns bytes
```

</details>

---

## BSON vs JSON

> Typed binary vs text-based, lossy representation

**Differences**

* BSON supports Date, Binary, Int32, Int64
* JSON has no native type metadata
* BSON preserves field order

<details>
<summary>Examples</summary>

```js
// JSON loses type information
JSON.stringify({ d: new Date() }); // date → string

// BSON preserves type
db.col.insertOne({ d: new Date(), n: NumberLong(10) });
```

</details>

---

## Data Types and Comparison Order

> Total ordering defined by BSON type precedence

**Comparison order (high → low)**

1. MinKey
2. Null
3. Numbers (int32, int64, double, decimal)
4. Symbol / String
5. Object
6. Array
7. BinData
8. ObjectId
9. Boolean
10. Date
11. Timestamp
12. Regex
13. MaxKey

<details>
<summary>Examples</summary>

```js
// Type-based ordering
db.t.find().sort({ v: 1 });

// Mixed-type values
{ v: null }
{ v: 1 }
{ v: '1' }
{ v: ObjectId() }
```

</details>

---

## ObjectId Internals

> 12-byte globally unique identifier

**Layout**

* 4 bytes: Unix timestamp
* 5 bytes: random (process + host)
* 3 bytes: incrementing counter

<details>
<summary>Examples</summary>

```js
const id = ObjectId();

id.getTimestamp();        // creation time
id.toHexString();         // 24-char hex
ObjectId.isValid(id);     // true

// Sort by creation time
db.col.find().sort({ _id: 1 });
```

</details>

---

## Null vs Missing Fields

> Semantically distinct but often conflated

**Behavior**

* `{ field: null }` matches both null and missing
* `$exists` differentiates

<details>
<summary>Examples</summary>

```js
// Matches null + missing
db.col.find({ a: null });

// Only missing
db.col.find({ a: { $exists: false } });

// Only explicit null
db.col.find({ a: null, a: { $exists: true } });
```

</details>

---

## Document Size Limits

> Hard 16MB BSON document limit

**Implications**

* Unbounded arrays are dangerous
* Large binary data belongs in GridFS

<details>
<summary>Examples</summary>

```js
// Check document size
Object.bsonsize(db.col.findOne());

// GridFS usage (large files)
// fs.files + fs.chunks collections
```

</details>

---

## Field Ordering and Semantics

> Field order is preserved and sometimes significant

**Where order matters**

* Index key patterns
* Compound `_id`
* Query shape caching

<details>
<summary>Examples</summary>

```js
// Field order preserved
{ a: 1, b: 2 } !== { b: 2, a: 1 }

// Index key order
db.col.createIndex({ a: 1, b: -1 }); // different from { b, a }

// _id compound ordering
{ _id: { a: 1, b: 2 } }
```

</details>

---

## Notes

* BSON favors **runtime efficiency over human readability**
* Type ordering directly affects indexes and sorts
* ObjectId enables locality-aware inserts

## Caveats

* Silent type coercion causes query mismatches
* Null handling is a frequent source of bugs
* Document bloat leads to performance cliffs
