---

id: mongodb-transactions-consistency
title: Transactions and Consistency
sidebar_position: 9
-------------------

# 9. Transactions and Consistency

> Guarantees around atomicity, isolation, and consistency in MongoDB

---

## Single-Document Atomicity

> Operations on a single document are atomic by default

**Interview points**

* Applies to insert, update, delete
* Covers embedded documents
* No partial updates

<details>
<summary>Examples (Node.js)</summary>

```js
// Atomic update on single document
await db.collection('accounts').updateOne(
  { _id: 1 },
  { $inc: { balance: -100 }, $set: { updatedAt: new Date() } }
);
```

</details>

---

## Multi-Document Transactions

> ACID transactions across multiple documents and collections

**Interview points**

* Requires replica set or sharded cluster
* Snapshot isolation
* All-or-nothing commit

<details>
<summary>Examples (Node.js)</summary>

```js
const session = client.startSession();

try {
  session.startTransaction();

  await db.collection('accounts').updateOne(
    { _id: 1 },
    { $inc: { balance: -100 } },
    { session }
  );

  await db.collection('accounts').updateOne(
    { _id: 2 },
    { $inc: { balance: 100 } },
    { session }
  );

  await session.commitTransaction();
} catch (e) {
  await session.abortTransaction();
} finally {
  await session.endSession();
}
```

</details>

---

## Sessions

> Logical context for causal consistency and transactions

**Interview points**

* Required for transactions
* Enables causal ordering
* Lightweight and client-managed

<details>
<summary>Examples (Node.js)</summary>

```js
const session = client.startSession({ causalConsistency: true });

await db.collection('users').findOne({ _id: 1 }, { session });

session.endSession();
```

</details>

---

## Retryable Writes

> Automatic retry of certain write operations

**Interview points**

* Enabled by default in drivers
* Prevents duplicate writes
* Requires `_id`

<details>
<summary>Examples (Node.js)</summary>

```js
const client = new MongoClient(uri, {
  retryWrites: true // boolean
});

await db.collection('logs').insertOne({ _id: 'x1', msg: 'ok' });
```

</details>

---

## Transaction Limits

> Hard limits enforced by the server

**Interview points**

* 60 seconds lifetime
* 100MB oplog entry limit
* No long-running transactions

<details>
<summary>Examples (Node.js)</summary>

```js
session.startTransaction({
  readConcern: { level: 'snapshot' }, // local | majority | snapshot
  writeConcern: { w: 'majority' },     // number | 'majority'
  readPreference: 'primary'            // primary only
});
```

</details>

---

## Performance Implications

> Cost of transactional guarantees

**Interview points**

* Slower than single-document ops
* Locks and retries
* Increased memory and oplog usage

<details>
<summary>Examples (Node.js)</summary>

```js
// Prefer atomic single-document model
await db.collection('orders').updateOne(
  { _id: 1, status: 'pending' },
  { $set: { status: 'paid' } }
);

// Avoid transaction if possible
```

</details>

---

## Notes

* Model data to avoid transactions
* Single-document atomicity is fastest

## Caveats

* Transactions reduce throughput
* Poor schema design forces transactions
* Not supported on standalone servers
