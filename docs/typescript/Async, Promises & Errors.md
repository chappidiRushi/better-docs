---

id: async-promises-errors
title: Async, Promises & Errors
sidebar_position: 12
-------------------

# Async, Promises & Errors

> Typing asynchronous flows and error handling in TypeScript

---

## Promise Typing

Defines the resolved value type of an asynchronous operation.

<details>
<summary>Examples</summary>

```ts
// basic promise
const p1: Promise<number> = Promise.resolve(1);

// promise with object
const p2: Promise<{ id: number; name: string }> = Promise.resolve({
  id: 1,
  name: 'Dev'
});

// function returning a promise
function fetchUser(): Promise<string> {
  return Promise.resolve('user');
}
```

</details>

---

## async / await

Provides synchronous-looking syntax for working with promises.

<details>
<summary>Examples</summary>

```ts
// async function always returns Promise<T>
async function getCount(): Promise<number> {
  return 10; // number → Promise<number>
}

// awaiting a promise
async function run() {
  const value = await getCount(); // number
}

// async arrow function
const load = async (): Promise<void> => {
  await Promise.resolve();
};
```

</details>

---

## Error Handling & unknown

Errors caught in `catch` blocks are typed as `unknown`.

<details>
<summary>Examples</summary>

```ts
try {
  throw new Error('fail');
} catch (err) {
  // err is unknown
  if (err instanceof Error) {
    console.log(err.message); // string
  }
}

// promise rejection
Promise.reject('error').catch((err: unknown) => {
  if (typeof err === 'string') {
    console.log(err);
  }
});
```

</details>

---

## try / catch Narrowing

Narrows error types using runtime checks.

<details>
<summary>Examples</summary>

```ts
function parse(input: string) {
  try {
    return JSON.parse(input) as object; // assertion
  } catch (err) {
    if (err instanceof SyntaxError) {
      return null;
    }

    if (typeof err === 'string') {
      return err;
    }

    throw err; // rethrow unknown
  }
}
```

</details>

---

## Notes

* `async` functions always return `Promise<T>`
* `await` unwraps the resolved value type
* `catch` variables are `unknown` by default
* Narrow errors before accessing properties
