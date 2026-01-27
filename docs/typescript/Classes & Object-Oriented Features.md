---
id: classes-object-oriented-features
title: Classes & Object-Oriented Features
sidebar_position: 7
---

# Classes & Object-Oriented Features

> TypeScript language support for class-based and object-oriented programming

---

## Classes & Constructors

Blueprints for creating objects with state and behavior, initialized via constructors.

<details>
<summary>Examples</summary>

```ts
class User {
  id: number;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
}

const user = new User(1, 'Dev');
```

</details>

---

## Parameter Properties

Shorthand syntax that declares and initializes class properties directly from constructor parameters.

<details>
<summary>Examples</summary>

```ts
class User {
  constructor(
    public id: number,           // public property
    private name: string,        // private property
    readonly active: boolean     // readonly property
  ) {}
}

const user = new User(1, 'Dev', true);
```

</details>

---

## Access Modifiers

Keywords that control visibility of class members.

<details>
<summary>Examples</summary>

```ts
class Service {
  public url: string;            // accessible everywhere
  protected token: string;       // accessible in subclasses
  private secret: string;        // accessible only in this class

  constructor(url: string) {
    this.url = url;
    this.token = 'token';
    this.secret = 'secret';
  }
}
```

</details>

---

## readonly Properties

Class properties that can be assigned only during initialization.

<details>
<summary>Examples</summary>

```ts
class Config {
  readonly env: string;

  constructor(env: string) {
    this.env = env;
  }
}

const cfg = new Config('prod');
// cfg.env = 'dev';               // ❌ cannot reassign
```

</details>

---

## Inheritance

Mechanism for extending a base class to reuse and specialize behavior.

<details>
<summary>Examples</summary>

```ts
class Animal {
  move() {
    return 'moving';
  }
}

class Dog extends Animal {
  bark() {
    return 'woof';
  }
}

const dog = new Dog();
dog.move();
dog.bark();
```

</details>

---

## Abstract Classes

Base classes that cannot be instantiated and may define abstract members.

<details>
<summary>Examples</summary>

```ts
abstract class Shape {
  abstract area(): number;       // must be implemented

  describe() {
    return 'shape';
  }
}

class Square extends Shape {
  constructor(private size: number) {
    super();
  }

  area() {
    return this.size * this.size;
  }
}
```

</details>

---

## implements

Enforces that a class satisfies a specific interface contract.

<details>
<summary>Examples</summary>

```ts
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message);
  }
}
```

</details>

---

## Static Members

Class-level properties and methods shared across all instances.

<details>
<summary>Examples</summary>

```ts
class MathUtil {
  static PI = 3.14;              // static property

  static square(value: number) {
    return value * value;        // static method
  }
}

MathUtil.PI;
MathUtil.square(4);
```

</details>

---

## Notes

* Classes are syntax sugar over JavaScript prototypes
* Access modifiers are compile-time only
* Abstract members enforce implementation in subclasses
* Static members belong to the class, not instances
