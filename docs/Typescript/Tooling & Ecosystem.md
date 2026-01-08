---

title: Tooling & Ecosystem
id: tooling-ecosystem
sidebar_position: 2
-------------------

# Tooling & Ecosystem

> Integrating TypeScript with linters, formatters, runtimes, and build tools

---

## ESLint with TypeScript

Adds static analysis and linting for TypeScript code.

<details>
<summary>Examples</summary>

```bash
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

```js
// .eslintrc.cjs
module.exports = {
  parser: '@typescript-eslint/parser',      // ESLint parser for TS
  plugins: ['@typescript-eslint'],          // TS-specific lint rules
  extends: [
    'eslint:recommended',
    'plugin:@typescript-eslint/recommended' // recommended TS ruleset
  ]
};
```

</details>

---

## Prettier

Formats code consistently without type awareness.

<details>
<summary>Examples</summary>

```bash
npm install --save-dev prettier
```

```jsonc
// .prettierrc
{
  "semi": true,            // boolean
  "singleQuote": true,    // boolean
  "trailingComma": "all" // 'none' | 'es5' | 'all'
}
```

</details>

---

## ts-node

Executes TypeScript files directly without precompilation.

<details>
<summary>Examples</summary>

```bash
npm install --save-dev ts-node
```

```bash
// run TS file directly
ts-node src/index.ts
```

```jsonc
// tsconfig.json
{
  "ts-node": {
    "transpileOnly": true // boolean (skip type checking)
  }
}
```

</details>

---

## Babel / SWC

Transpiles TypeScript without type checking.

<details>
<summary>Examples</summary>

```bash
npm install --save-dev @babel/core @babel/preset-typescript
```

```js
// babel.config.cjs
module.exports = {
  presets: [
    '@babel/preset-typescript' // strips types only
  ]
};
```

```bash
# SWC example
npm install --save-dev @swc/core @swc/cli
```

```jsonc
// .swcrc
{
  "jsc": {
    "parser": {
      "syntax": "typescript" // 'typescript'
    }
  }
}
```

</details>

---

## Build Tools Integration

Integrates TypeScript into bundlers and task runners.

<details>
<summary>Examples</summary>

```ts
// Vite / ESBuild style usage
import { defineConfig } from 'vite';

export default defineConfig({
  esbuild: {
    target: 'es2020' // string (ES target)
  }
});
```

```js
// Webpack loader example
module.exports = {
  module: {
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',          // transpile + type check
        exclude: /node_modules/
      }
    ]
  }
};
```

</details>

---

## Notes

* ESLint handles static analysis, not formatting
* Prettier handles formatting, not types
* ts-node is best for development and scripts
* Babel and SWC are faster but skip type checking
* Build tools delegate type checking to `tsc` when needed
