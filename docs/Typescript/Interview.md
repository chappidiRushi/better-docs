---

title: TypeScript Interview Questions
sidebar_position: 18
---

# TypeScript Interview Questions & Answers

---

## Fundamentals (1–20)

1. **What is TypeScript?**
   TypeScript is a statically typed superset of JavaScript that adds compile-time type checking.

2. **Is TypeScript compiled or interpreted?**
   TypeScript is compiled to JavaScript using the TypeScript compiler.

3. **Does TypeScript run in the browser?**
   No, browsers run JavaScript; TypeScript must be compiled first.

4. **What file extension does TypeScript use?**
   `.ts` for TypeScript and `.tsx` for TS with JSX.

5. **What is static typing?**
   Types are checked at compile time instead of runtime.

6. **What are the benefits of TypeScript?**
   Type safety, better tooling, improved maintainability, and early error detection.

7. **What is type inference?**
   TypeScript automatically determines types based on assigned values.

8. **What is a type annotation?**
   Explicitly specifying a type for a variable, parameter, or return value.

9. **What is `any`?**
   A type that disables type checking.

10. **What is `unknown`?**
    A safer alternative to `any` that requires type narrowing.

11. **What is `void`?**
    Represents absence of a return value.

12. **What is `never`?**
    Represents values that never occur.

13. **What is a tuple?**
    An array with fixed length and known element types.

14. **What is an enum?**
    A set of named numeric or string constants.

15. **What is structural typing?**
    Type compatibility based on shape rather than explicit declaration.

16. **What is strict mode?**
    A compiler option enabling strict type checks.

17. **What is `tsconfig.json`?**
    Configuration file for the TypeScript compiler.

18. **What does `noImplicitAny` do?**
    Disallows implicit `any` types.

19. **What is `readonly`?**
    Prevents reassignment after initialization.

20. **What is `as const`?**
    Narrows values to literal and readonly types.

---

## Types & Narrowing (21–45)

1. **What are union types?**
    Types that can be one of several types.

2. **What are intersection types?**
    Combine multiple types into one.

3. **What is a literal type?**
    A type with a specific value.

4. **What is type narrowing?**
    Reducing a broader type to a more specific one.

5. **What is `typeof` used for?**
    Extracting types from values.

6. **What is `instanceof` used for?**
    Runtime checking of class instances.

7. **What is a type guard?**
    A runtime check that narrows a type.

8. **What is a user-defined type guard?**
    A function that returns a type predicate.

9. **What are discriminated unions?**
    Unions differentiated by a common literal property.

10. **What is control-flow analysis?**
    Type narrowing based on code paths.

11. **Difference between `any` and `unknown`?**
    `unknown` requires type checks before use.

12. **What is `null` vs `undefined`?**
    `null` is explicit absence; `undefined` is uninitialized.

13. **What does `strictNullChecks` do?**
    Prevents assigning null/undefined where not allowed.

14. **What is optional chaining?**
    Safely accesses nested properties.

15. **What is non-null assertion (`!`)?**
    Tells the compiler a value is not null.

16. **What is `keyof`?**
    Produces a union of object keys.

17. **What is indexed access type?**
    Accesses a property type via indexing.

18. **What is `satisfies`?**
    Ensures a value conforms to a type without widening.

19. **What is a recursive type?**
    A type that references itself.

20. **What is `infer`?**
    Captures a type inside conditional types.

21. **What is a conditional type?**
    Type selected based on a condition.

22. **What is distributive conditional type?**
    Conditional types applied to union members.

23. **What is template literal type?**
    Creates string types using template syntax.

24. **What is `NonNullable`?**
    Removes null and undefined.

25. **What is `Exclude`?**
    Removes union members.

---

## Interfaces & Objects (46–65)

1. **What is an interface?**
    A contract defining object shape.

2. **Interface vs type alias?**
    Interfaces can merge; types cannot.

3. **What is declaration merging?**
    Multiple interface declarations combine.

4. **What is optional property?**
    Property that may be absent.

5. **What is readonly property?**
    Property that cannot be reassigned.

6. **What are index signatures?**
    Allow dynamic property keys.

7. **What is excess property check?**
    Prevents extra properties in object literals.

8. **What is `Record`?**
    Creates object types with specific keys.

9. **What is `Pick`?**
    Selects specific properties.

10. **What is `Omit`?**
    Removes specific properties.

11. **What is `Partial`?**
    Makes all properties optional.

12. **What is `Required`?**
    Makes all properties required.

13. **What is `Readonly<T>`?**
    Makes all properties immutable.

14. **What is call signature?**
    Defines function shape inside object.

15. **What is construct signature?**
    Defines constructor shape.

16. **What is `this` typing?**
    Explicitly types `this` in functions.

17. **What is object literal typing?**
    Typing inline object definitions.

18. **What is structural compatibility?**
    Assignment based on property shape.

19. **What is `exactOptionalPropertyTypes`?**
    Differentiates missing vs undefined.

20. **What is mapped type?**
    Transforms properties of another type.

---

## Functions & Generics (66–95)

1. **What is a function type?**
    Type describing function parameters and return.

2. **What are optional parameters?**
    Parameters that may be omitted.

3. **What are default parameters?**
    Parameters with default values.

4. **What are rest parameters?**
    Collects remaining arguments.

5. **What is function overload?**
    Multiple call signatures for one function.

6. **What are generics?**
    Types parameterized over other types.

7. **Why use generics?**
    Preserve type relationships.

8. **What is generic constraint?**
    Limits acceptable generic types.

9. **What is default generic parameter?**
    Fallback type for generics.

10. **What is `keyof` with generics?**
    Constrains keys to object properties.

11. **What is variance?**
    Rules for type assignability.

12. **What is covariance?**
    Output-only assignability.

13. **What is contravariance?**
    Input-only assignability.

14. **What is invariance?**
    No assignability.

15. **What is generic interface?**
    Interface with type parameters.

16. **What is generic class?**
    Class parameterized by types.

17. **What is higher-order generic?**
    Generic returning generic.

18. **What is `ReturnType`?**
    Extracts function return type.

19. **What is `Parameters`?**
    Extracts parameter tuple.

20. **What is `InstanceType`?**
    Extracts instance type.

21. **What is `Awaited`?**
    Unwraps Promise types.

22. **What is type assertion?**
    Overrides inferred type.

23. **`as` vs angle bracket assertion?**
    `as` is preferred and JSX-safe.

24. **What is overload signature vs implementation?**
    Implementation must cover all overloads.

25. **What is contextual typing?**
    Type inference from context.

26. **What is generic factory?**
    Function producing typed instances.

27. **What is `ThisType`?**
    Contextual `this` typing.

28. **What is function bivariance?**
    Special assignability rule for callbacks.

29. **What is `never` in functions?**
    Functions that never return.

30. **What is exhaustiveness checking?**
    Ensuring all cases are handled.

---

## Classes, Modules & Tooling (96–150)

1. **What is a class in TypeScript?**
    Blueprint for creating objects.

2. **What are parameter properties?**
    Constructor shorthand for properties.

3. **What are access modifiers?**
    public, private, protected visibility.

4. **What is `implements`?**
    Class conforms to interface.

5. **What is `abstract` class?**
     Base class that cannot be instantiated.

6. **What are static members?**
     Belong to class, not instances.

7. **What is module?**
     File with import/export.

8. **Namespace vs module?**
     Namespaces are global; modules are scoped.

9. **What is path mapping?**
     Alias module paths via tsconfig.

10. **What is ES module?**
     Standard JavaScript module system.

11. **What is dynamic import?**
     Runtime module loading.

12. **What is declaration file?**
     `.d.ts` type-only file.

13. **What is ambient declaration?**
     Declares existing global types.

14. **What is DefinitelyTyped?**
     Community typings repository.

15. **What is `@types`?**
     Installed type definitions.

16. **What is JavaScript interop?**
     Using TS with JS code.

17. **What is JSDoc typing?**
     Type-check JS via comments.

18. **What is `ts-node`?**
     Runs TS directly.

19. **What is Babel role?**
     Transpiles without type checking.

20. **What is SWC?**
     Fast TS/JS compiler.

21. **What is ESLint role?**
     Static code analysis.

22. **What is Prettier role?**
     Code formatting.

23. **What is incremental build?**
     Speeds up recompilation.

24. **What is composite project?**
     Project reference-ready build.

25. **What is project reference?**
     Links TS projects.

26. **What is module resolution?**
     How imports map to files.

27. **Node vs classic resolution?**
     Different lookup strategies.

28. **What is `lib` option?**
     Includes built-in typings.

29. **What is `target` option?**
     JS version output.

30. **What is `emitDecoratorMetadata`?**
     Emits metadata for decorators.

31. **What is `skipLibCheck`?**
     Skips type checking of libs.

32. **What is `isolatedModules`?**
     Ensures per-file transpilation.

33. **What is `useDefineForClassFields`?**
     Controls class field emit.

34. **What is `esModuleInterop`?**
     Improves CommonJS interop.

35. **What is `forceConsistentCasingInFileNames`?**
     Enforces import casing.

36. **What is `allowJs`?**
     Allows JS files.

37. **What is `checkJs`?**
     Type-check JS files.

38. **What is `resolveJsonModule`?**
     Allows importing JSON.

39. **What is `noEmit`?**
     Prevents output.

40. **What is `emitDeclarationOnly`?**
     Only emits `.d.ts`.

41. **What is `downlevelIteration`?**
     Improves ES5 iteration.

42. **What is `sourceMap`?**
     Maps JS to TS.

43. **What is `declarationMap`?**
     Maps `.d.ts` to TS.

44. **What is `strictFunctionTypes`?**
     Checks function assignability.

45. **What is `exactOptionalPropertyTypes`?**
     Strict optional semantics.

46. **What is `noUncheckedIndexedAccess`?**
     Adds undefined to index access.

47. **What is `useUnknownInCatchVariables`?**
     Types catch as unknown.

48. **What is `verbatimModuleSyntax`?**
     Preserves import/export.

49. **What is `preserveValueImports`?**
     Keeps type-only imports.

50. **What is `importsNotUsedAsValues`?**
     Controls import elision.

51. **What is `allowImportingTsExtensions`?**
     Allows `.ts` in imports.

52. **What is `moduleDetection`?**
     Controls module classification.

53. **What is `noImplicitOverride`?**
     Requires override keyword.

54. **What is `useDefineForClassFields` impact?**
     Aligns with JS spec.

55. **What is best TS interview advice?**
     Explain trade-offs and real usage.
