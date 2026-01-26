---

id: mongoose-transactions-sessions
title: Transactions and Sessions
sidebar_position: 35
--------------------

# Transactions and Sessions

> Multi-document ACID transactions using MongoDB sessions in Mongoose

---

## 1. Sessions in Mongoose

> Logical context for grouping operations; required for transactions

```js
mongoose.startSession(options?)      // → Promise<ClientSession>
```

<details>
<summary>Examples</summary>

```js
const session = await mongoose.startSession({
  causalConsistency: true           // boolean
});

// attach to queries
await User.findOne({ _id: id }).session(session);

// pass explicitly
await Order.create([{ total: 100 }], { session });
```

</details>

---

## 2. Transaction Lifecycle

> Explicit control over transaction boundaries

```js
session.startTransaction(options?)
session.commitTransaction()
session.abortTransaction()
session.endSession()
```

<details>
<summary>Examples</summary>

```js
const session = await mongoose.startSession();

try {
  session.startTransaction({
    readConcern: { level: 'snapshot' }, // 'local' | 'majority' | 'snapshot'
    writeConcern: { w: 'majority' },    // number | 'majority'
    readPreference: 'primary'           // string
  });

  await User.updateOne(
    { _id: userId },
    { $inc: { balance: -100 } },
    { session }
  );

  await Order.create([
    { userId, amount: 100 }
  ], { session });

  await session.commitTransaction();
} catch (err) {
  await session.abortTransaction();
  throw err;
} finally {
  session.endSession();
}
```

</details>

---

## 3. Error Handling

> Handle transient transaction errors explicitly

```js
error.hasErrorLabel('TransientTransactionError')
error.hasErrorLabel('UnknownTransactionCommitResult')
```

<details>
<summary>Examples</summary>

```js
try {
  await session.commitTransaction();
} catch (err) {
  if (err.hasErrorLabel('UnknownTransactionCommitResult')) {
    // commit outcome unknown
  }
  if (err.hasErrorLabel('TransientTransactionError')) {
    // safe to retry entire transaction
  }
  throw err;
}
```

</details>

---

## 4. Retry Strategies

> Idempotent retries for transient failures

```js
withTransaction(fn, options?)
```

<details>
<summary>Examples</summary>

```js
const session = await mongoose.startSession();

await session.withTransaction(async () => {
  await User.updateOne(
    { _id: userId },
    { $inc: { balance: -50 } },
    { session }
  );

  await Ledger.create([
    { userId, delta: -50 }
  ], { session });
}, {
  readConcern: { level: 'snapshot' },
  writeConcern: { w: 'majority' },
  readPreference: 'primary'
});

session.endSession();
```

</details>

---

## Notes

* Transactions require replica set or sharded cluster
* All operations must use the same session
* `withTransaction` handles retries automatically

## Caveats

* No transactions on standalone MongoDB
* Long transactions increase lock pressure
* Non-idempotent operations complicate retries
