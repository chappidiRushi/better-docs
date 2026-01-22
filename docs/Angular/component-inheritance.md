---
id: component-inheritance
title: Component Inheritance
sidebar_position: 11
---

# Component Inheritance

> How components can reuse logic by extending base classes

---

## What is Component Inheritance

Component inheritance allows one component class to **extend another class** and reuse its properties, methods, and behavior.

Angular treats inheritance as **plain TypeScript inheritance**.

Only the **derived class** is an Angular component.

---

## Base Class (Non-Component)

A base class contains **shared logic only**.

It does not have Angular metadata and is never rendered.

<details>
<summary>Example</summary>

```ts
export abstract class BaseList {
  items: string[] = [];

  add(item: string) {
    this.items.push(item);
  }
}
```

</details>

---

## Extending a Base Class

A component extends the base class using `extends`.

The component inherits all public and protected members.

<details>
<summary>Example</summary>

```ts
@Component({
  selector: 'app-todo',
  standalone: true,
  template: `{{ items.length }}`
})
export class TodoComponent extends BaseList {
  constructor() {
    super();
    this.add('Task 1');
  }
}
```

</details>

---

## Inheritance with Dependency Injection

Base classes can use Angular DI via `inject()`.

Dependencies are resolved from the derived component’s injector.

<details>
<summary>Example</summary>

```ts
export abstract class BaseLogger {
  protected logger = inject(LoggerService);

  log(msg: string) {
    this.logger.log(msg);
  }
}

@Component({
  selector: 'app-user',
  standalone: true,
  template: `User`
})
export class UserComponent extends BaseLogger {
  save() {
    this.log('saved');
  }
}
```

</details>

---

## Lifecycle Hooks and Inheritance

Lifecycle hooks **are not inherited automatically**.

If both base and child implement the same hook, the child must call `super`.

<details>
<summary>Example</summary>

```ts
export abstract class BaseLifecycle {
  ngOnInit() {
    console.log('base init');
  }
}

@Component({
  selector: 'app-demo',
  standalone: true
})
export class DemoComponent extends BaseLifecycle implements OnInit {
  override ngOnInit() {
    super.ngOnInit();
    console.log('child init');
  }
}
```

</details>

---

## Abstract Base Components

Base classes are often marked `abstract` to prevent direct instantiation.

They define **contracts** for derived components.

<details>
<summary>Example</summary>

```ts
export abstract class BaseForm {
  abstract submit(): void;
}

@Component({
  selector: 'app-login',
  standalone: true
})
export class LoginComponent extends BaseForm {
  submit() {}
}
```

</details>

---

## What Is Not Inherited

The following are **not inherited**:

* `@Component()` metadata
* templates and styles
* selectors
* providers
  nEach component must declare its own Angular metadata.

---

## When to Use Inheritance

Use inheritance when:

* behavior is truly shared
* the relationship is *is-a*
* logic cannot be composed cleanly

Avoid inheritance for:

* UI composition
* cross-cutting concerns
* reusable view structure

---

## Prefer Composition When Possible

In many cases, **composition is simpler and safer** than inheritance.

Examples of composition:

* services
* directives
* standalone utility functions

---

## Deprecated Pattern: Base Class as Component

> ⚠️ Discouraged pattern

Using a decorated component as a base class leads to unclear metadata behavior.

<details>
<summary>Example</summary>

```ts
@Component({ selector: 'base' })
export class BaseComponent {}

@Component({ selector: 'child' })
export class ChildComponent extends BaseComponent {}
```

</details>

---

## Mental Model to Remember

* Inheritance is **TypeScript-only**
* Angular sees only the final component
* Metadata is **never inherited**
* Use inheritance for logic, not structure
