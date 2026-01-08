---
sidebar_position: 19
---


# Glossary

---

## A

**Abstract Class** — A class that cannot be instantiated and is intended to be extended.

**Access Modifier** — Keyword controlling visibility (`public`, `protected`, `private`).

**Ambient Declaration** — A declaration that describes existing variables or modules without implementation.

**any** — A type that disables all type checking.

**Assertion Function** — A function that asserts a condition and narrows types using `asserts`.

**Awaited&lt;T>** — Utility type that unwraps nested `Promise` types.

---

## B

**BigInt** — Primitive type for arbitrarily large integers.

**Bivariance** — Special variance rule applied to function parameters in callbacks.

---

## C

**Call Signature** — Type definition for a callable object.

**Catch Clause Narrowing** — Type narrowing inside `catch` blocks using `unknown`.

**Class** — Blueprint for creating objects with properties and methods.

**Composite Project** — A TypeScript project configured for project references.

**Conditional Type** — A type resolved based on a condition (`T extends U ? X : Y`).

**Const Assertion (`as const`)** — Prevents literal widening and makes properties readonly.

**Control Flow Analysis** — Compiler analysis that narrows types based on code paths.

---

## D

**Declaration File (`.d.ts`)** — File that contains only type declarations.

**Declaration Merging** — Combining multiple declarations with the same name.

**DefinitelyTyped** — Community repository hosting type definitions.

**Discriminated Union** — Union type distinguished by a shared literal property.

**Downlevel Iteration** — Compiler support for iterables in older JS targets.

---

## E

**Enum** — A set of named constants (numeric or string-based).

**Excess Property Check** — Validation that prevents extra properties on object literals.

**Exclude&lt;T, U>** — Removes members of `U` from union `T`.

---

## F

**Function Overload** — Multiple call signatures for a single function implementation.

**Function Type** — Type describing parameters and return value of a function.

---

## G

**Generic** — A type parameter that makes components reusable across types.

**Generic Constraint** — Restriction applied to a generic type parameter.

**Global Scope** — Declarations available everywhere without imports.

---

## I

**implements** — Keyword enforcing class conformance to an interface.

**Index Signature** — Allows arbitrary property keys with a defined value type.

**Indexed Access Type** — Retrieves property types via indexing (`T[K]`).

**infer** — Keyword to infer types within conditional types.

**Intersection Type (`&`)** — Combines multiple types into one.

**Interface** — Structural contract defining object shape.

**Isolated Modules** — Compiler mode ensuring files can be transpiled independently.

---

## J

**JSDoc Typing** — Using comments to add types in JavaScript files.

---

## K

**keyof** — Produces a union of property names of a type.

---

## L

**Literal Type** — Type representing a specific value.

**lib** — Compiler option specifying built-in type libraries.

---

## M

**Mapped Type** — Type created by transforming properties of another type.

**Module** — A file with import/export statements.

**Module Resolution** — Strategy for resolving import paths.

---

## N

**Namespace** — Internal module for grouping declarations (legacy).

**never** — Type representing values that never occur.

**NonNullable&lt;T>** — Removes `null` and `undefined` from a type.

---

## O

**Object Type Literal** — Inline object type definition.

**Optional Property** — Property that may be omitted.

**Omit&lt;T, K>** — Removes specified keys from a type.

---

## P

**Partial&lt;T>** — Makes all properties optional.

**Path Mapping** — Alias configuration for module imports.

**Primitive Type** — Built-in simple types (`string`, `number`, `boolean`, etc.).

**Project Reference** — Dependency between TypeScript projects.

---

## R

**Readonly&lt;T>** — Makes all properties immutable.

**ReadonlyArray&lt;T>** — Immutable array type.

**Record&lt;K, T>** — Object type with keys `K` and values `T`.

**Recursive Type** — Type that references itself.

---

## S

**satisfies** — Ensures a value conforms to a type without widening.

**Source Map** — Maps generated JS back to TS source.

**Static Member** — Class member belonging to the class itself.

**Strict Mode** — Set of compiler options enabling strict checks.

**Structural Typing** — Type compatibility based on shape, not name.

---

## T

**Template Literal Type** — Constructs string types using template syntax.

**this Parameter** — Explicit typing of `this` in functions.

**Tuple** — Fixed-length array with known element types.

**Type Alias** — Named type definition.

**Type Annotation** — Explicit type declaration.

**Type Assertion** — Forcing the compiler to treat a value as a specific type.

**Type Guard** — Runtime check that narrows a type.

**Type Inference** — Automatic deduction of types by the compiler.

---

## U

**Union Type (`|`)** — Type that can be one of multiple types.

**unknown** — Safe top type requiring narrowing before use.

**Utility Type** — Built-in generic helper types.

---

## V

**Variance** — Rules for type assignability in generics.

**void** — Represents absence of a return value.

---

## W

**Widening** — Process of converting literal types to broader types.

---

## Z

**Zero-cost Abstraction** — Type-level features erased at runtime.
