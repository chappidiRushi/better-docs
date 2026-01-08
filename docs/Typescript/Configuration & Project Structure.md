---

id: configuration-Project_structure
title:  Configuration & Project Structure
sidebar_position: 14
-------------------

# Configuration & Project Structure

> Configuring TypeScript projects, builds, and module resolution

---

## Strict Type-Checking Options

Enables stricter compile-time checks to catch potential errors early.

<details>
<summary>Examples</summary>

```jsonc
{
  "compilerOptions": {
    // Enables all strict type-checking options
    "strict": true,                // boolean (true | false)

    // Disallows implicit `any` types
    "noImplicitAny": true,         // boolean

    // Requires strict null and undefined checks
    "strictNullChecks": true,      // boolean

    // Ensures `this` is properly typed
    "noImplicitThis": true,        // boolean

    // Enables strict checking of function types
    "strictFunctionTypes": true,   // boolean

    // Checks class properties for definite assignment
    "strictPropertyInitialization": true // boolean
  }
}
```

</details>

---

## Incremental Builds

Stores build information to speed up subsequent compilations.

<details>
<summary>Examples</summary>

```jsonc
{
  "compilerOptions": {
    // Enables incremental compilation
    "incremental": true,           // boolean

    // Location of incremental build info file
    "tsBuildInfoFile": "./.tsbuildinfo" // string
  }
}
```

</details>

---

## Composite Projects

Marks a project as buildable and referenceable by other projects.

<details>
<summary>Examples</summary>

```jsonc
{
  "compilerOptions": {
    // Enables project references
    "composite": true,             // boolean

    // Emits declaration files
    "declaration": true,           // boolean

    // Emits declaration maps
    "declarationMap": true         // boolean
  }
}
```

</details>

---

## Project References

Allows splitting large codebases into multiple dependent projects.

<details>
<summary>Examples</summary>

```jsonc
// tsconfig.json
{
  "references": [
    { "path": "./core" },         // path to referenced project
    { "path": "./shared" }
  ]
}
```

```jsonc
// core/tsconfig.json
{
  "compilerOptions": {
    "composite": true              // boolean
  }
}
```

</details>

---

## Module Resolution Strategies

Controls how module imports are resolved to files.

<details>
<summary>Examples</summary>

```jsonc
{
  "compilerOptions": {
    // Module resolution strategy
    "moduleResolution": "node",   // 'node' | 'classic' | 'bundler'

    // Base directory for non-relative imports
    "baseUrl": "./src",           // string

    // Path alias mappings
    "paths": {
      "@utils/*": ["utils/*"],    // string[]
      "@models/*": ["models/*"]  // string[]
    },

    // File extensions to resolve
    "resolveJsonModule": true,     // boolean
    "allowImportingTsExtensions": false // boolean
  }
}
```

```ts
import { sum } from '@utils/math';
```

</details>

---

## Notes

* Strict options improve type safety but may increase initial friction
* Incremental builds significantly speed up large projects
* Composite projects are required for project references
* Project references enable scalable monorepo setups
* Module resolution affects how imports map to files
