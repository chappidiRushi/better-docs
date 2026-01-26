---

id: mongoose-testing-tooling
title: Testing and Tooling
sidebar_position: 41
--------------------

# Testing and Tooling

> Reliable test environments, mocking strategies, schema assertions, and performance validation

---

## 1. In-Memory MongoDB

> Ephemeral MongoDB instances for fast, isolated tests

```js
mongodb-memory-server
```

<details>
<summary>Examples</summary>

```js
import { MongoMemoryServer } from 'mongodb-memory-server';

let mongo;

beforeAll(async () => {
  mongo = await MongoMemoryServer.create({
    binary: { version: '6.0.0' }   // string
  });

  await mongoose.connect(mongo.getUri(), {
    dbName: 'test',                // string
    autoIndex: true                // boolean
  });
});

afterAll(async () => {
  await mongoose.disconnect();
  await mongo.stop();
});
```

</details>

---

## 2. Mocking Mongoose

> Isolate business logic without hitting MongoDB

```js
jest.spyOn | sinon.stub
```

<details>
<summary>Examples</summary>

```js
// mock model method
jest.spyOn(User, 'findOne').mockResolvedValue({
  _id: '1',
  email: 'a@test.com'
});

// mock document method
jest.spyOn(User.prototype, 'save').mockResolvedValue();

// restore
jest.restoreAllMocks();
```

</details>

---

## 3. Schema Testing

> Assert schema constraints and behavior

```js
validate | required | indexes | middleware
```

<details>
<summary>Examples</summary>

```js
// validation
const user = new User({});

const err = user.validateSync();
err.errors.email;                // required

// index existence
const indexes = User.schema.indexes();
indexes.some(([keys]) => keys.email === 1);

// middleware execution
const spy = jest.fn();
schema.pre('save', spy);
await new User({ email: 'a@test.com' }).save();
expect(spy).toHaveBeenCalled();
```

</details>

---

## 4. Performance Testing

> Detect regressions and slow queries

```js
benchmark | explain | load tests
```

<details>
<summary>Examples</summary>

```js
// query timing
console.time('query');
await User.find({ active: true }).lean();
console.timeEnd('query');

// explain plan
await User.find({ email })
  .explain('executionStats');

// basic load loop
for (let i = 0; i < 1e4; i++) {
  await User.findOne({ email }).lean();
}
```

</details>

---

## Notes

* In-memory MongoDB is for tests only
* Mocking trades realism for speed
* Schema tests catch regressions early

## Caveats

* In-memory server behavior can differ from prod
* Excessive mocking hides integration issues
* Performance tests must reflect real indexes
