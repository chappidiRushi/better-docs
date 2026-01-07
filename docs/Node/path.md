# `path` Module

> File system path utilities (platform‑aware)

---

## Import (ESM)

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## Platform Info

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## Path Resolution

```js
// Resolve paths into an absolute path (rightmost absolute wins)
path.resolve(...paths)            // (...string[]) → string
// Normalize path by resolving '..', '.', and duplicate separators
path.normalize(path)              // (string) → string
// Check if path is absolute
path.isAbsolute(path)             // (string) → boolean
// Join path segments safely
path.join(...paths)               // (...string[]) → string
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## Parsing / Formatting

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## Path Parts

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## Relative Paths

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## URL Conversion

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## POSIX / Win32 Explicit APIs

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

<details>
<summary>Examples</summary>

```js
// Platform-specific path separators
path.sep                         // string — path segment separator
path.delimiter                   // string — PATH environment delimiter
path.win32                       // Windows-specific path API
path.posix                       // POSIX-specific path API
```

</details>

---

## Notes

* `resolve` uses **cwd** implicitly
* `join` does not normalize absolute roots
* Prefer `posix` / `win32` for deterministic output
