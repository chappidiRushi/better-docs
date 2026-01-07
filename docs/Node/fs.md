# `fs` Module

> File system APIs — **sync + async + streams**, core module

---

## Import (ESM)

```js
import fs from 'node:fs';
```

<details>
<summary>Examples</summary>

```js
// import fs from 'node:fs'
```

</details>

---

## File Reading

```js
fs.readFile(path, opts?, cb)        // (string|URL, object|string?, (err, data)=>void) → void
fs.readFileSync(path, opts?)        // (string|URL, object|string?) → Buffer|string
fs.createReadStream(path, opts?)    // (string|URL, object?) → ReadStream
```

<details>
<summary>Examples</summary>

```js
fs.readFile('a.txt', 'utf8', (err, data) => {});          // path, encoding, callback
fs.readFileSync('a.txt', { encoding: 'utf8' });          // path, options
fs.createReadStream('a.txt', { highWaterMark: 64 });     // path, stream options
```

</details>

---

## File Writing

```js
fs.writeFile(path, data, opts?, cb)  // (string|URL, any, object|string?, fn) → void
fs.writeFileSync(path, data, opts?)  // (string|URL, any, object|string?) → void
fs.appendFile(path, data, opts?, cb) // (string|URL, any, object|string?, fn) → void
fs.appendFileSync(path, data, opts?) // (string|URL, any, object|string?) → void
fs.createWriteStream(path, opts?)    // (string|URL, object?) → WriteStream
```

<details>
<summary>Examples</summary>

```js
fs.writeFile('a.txt', 'hello', 'utf8', () => {});         // path, data, encoding, cb
fs.writeFileSync('a.txt', 'hello', { flag: 'w' });       // path, data, options
fs.appendFile('a.txt', '!', () => {});                   // path, data, cb
fs.appendFileSync('a.txt', '!', 'utf8');                 // path, data, encoding
fs.createWriteStream('a.txt', { flags: 'a' });           // path, stream options
```

</details>

---

## File Info / Metadata

```js
fs.stat(path, cb)                    // (string|URL, (err, Stats)=>void) → void
fs.statSync(path)                    // (string|URL) → Stats
fs.lstat(path, cb)                   // (string|URL, (err, Stats)=>void) → void
fs.lstatSync(path)                   // (string|URL) → Stats
fs.access(path, mode?, cb)           // (string|URL, number?, fn) → void
fs.accessSync(path, mode?)           // (string|URL, number?) → void
```

<details>
<summary>Examples</summary>

```js
fs.stat('a.txt', (err, s) => s.isFile());                 // path, cb
fs.statSync('a.txt').size;                               // path
fs.lstatSync('link.txt').isSymbolicLink();               // path
fs.access('a.txt', fs.constants.R_OK, () => {});         // path, mode, cb
fs.accessSync('a.txt', fs.constants.W_OK);               // path, mode
```

</details>

---

## Paths & Links

```js
fs.realpath(path, opts?, cb)         // (string|URL, object?, fn) → void
fs.realpathSync(path, opts?)         // (string|URL, object?) → string
fs.readlink(path, opts?, cb)         // (string|URL, object?, fn) → void
fs.readlinkSync(path, opts?)         // (string|URL, object?) → string
fs.symlink(target, path, type?, cb)  // (string, string, string?, fn) → void
fs.symlinkSync(target, path, type?)  // (string, string, string?) → void
```

<details>
<summary>Examples</summary>

```js
fs.realpath('link.txt', 'utf8', () => {});               // path, encoding, cb
fs.realpathSync('link.txt');                            // path
fs.readlinkSync('link.txt', 'utf8');                    // path, encoding
fs.symlink('a.txt', 'link.txt', 'file', () => {});      // target, path, type, cb
fs.symlinkSync('a.txt', 'link2.txt');                   // target, path
```

</details>

---

## Directories

```js
fs.mkdir(path, opts?, cb)             // (string|URL, object?, fn) → void
fs.mkdirSync(path, opts?)             // (string|URL, object?) → void
fs.readdir(path, opts?, cb)           // (string|URL, object?, fn) → void
fs.readdirSync(path, opts?)           // (string|URL, object?) → string[]
fs.rm(path, opts?, cb)                // (string|URL, object?, fn) → void
fs.rmSync(path, opts?)                // (string|URL, object?) → void
```

<details>
<summary>Examples</summary>

```js
fs.mkdir('dir', { recursive: true }, () => {});          // path, options, cb
fs.mkdirSync('dir2');                                   // path
fs.readdir('.', { withFileTypes: true }, () => {});     // path, options, cb
fs.readdirSync('.');                                    // path
fs.rm('dir', { recursive: true }, () => {});            // path, options, cb
fs.rmSync('dir2');                                      // path
```

</details>

---

## File Manipulation

```js
fs.rename(oldPath, newPath, cb)       // (string|URL, string|URL, fn) → void
fs.renameSync(oldPath, newPath)       // (string|URL, string|URL) → void
fs.copyFile(src, dest, mode?, cb)     // (string|URL, string|URL, number?, fn) → void
fs.copyFileSync(src, dest, mode?)     // (string|URL, string|URL, number?) → void
fs.truncate(path, len?, cb)           // (string|URL, number?, fn) → void
fs.truncateSync(path, len?)           // (string|URL, number?) → void
```

<details>
<summary>Examples</summary>

```js
fs.rename('a.txt', 'b.txt', () => {});                   // oldPath, newPath, cb
fs.renameSync('b.txt', 'c.txt');                         // oldPath, newPath
fs.copyFile('c.txt', 'd.txt', () => {});                 // src, dest, cb
fs.copyFileSync('d.txt', 'e.txt');                       // src, dest
fs.truncate('e.txt', 10, () => {});                      // path, length, cb
fs.truncateSync('e.txt', 5);                             // path, length
```

</details>

---

## File Descriptors

```js
fs.open(path, flags, mode?, cb)       // (string|URL, string, number?, fn) → void
fs.openSync(path, flags, mode?)       // (string|URL, string, number?) → number
fs.close(fd, cb)                      // (number, fn) → void
fs.closeSync(fd)                      // (number) → void
```

<details>
<summary>Examples</summary>

```js
fs.open('a.txt', 'r', (err, fd) => {});                  // path, flags, cb
const fd = fs.openSync('a.txt', 'r');                    // path, flags
fs.close(fd, () => {});                                  // fd, cb
fs.closeSync(fd);                                        // fd
```

</details>

---

## Watching

```js
fs.watch(path, opts?, listener?)      // (string|URL, object?, fn?) → FSWatcher
fs.watchFile(path, opts?, listener)   // (string|URL, object?, fn) → void
fs.unwatchFile(path, listener?)       // (string|URL, fn?) → void
```

<details>
<summary>Examples</summary>

```js
fs.watch('a.txt', { persistent: true }, (e, f) => {});  // path, options, listener
fs.watchFile('a.txt', { interval: 500 }, () => {});     // path, options, listener
fs.unwatchFile('a.txt');                                // path
```

</details>

---

## Constants

```js
// Access permissions (fs.access)
fs.constants.F_OK          // file exists
fs.constants.R_OK          // readable
fs.constants.W_OK          // writable
fs.constants.X_OK          // executable

// Open flags (fs.open)
fs.constants.O_RDONLY      // read only
fs.constants.O_WRONLY      // write only
fs.constants.O_RDWR        // read & write
fs.constants.O_CREAT       // create if not exists
fs.constants.O_EXCL        // exclusive create
fs.constants.O_NOCTTY      // no controlling terminal
fs.constants.O_TRUNC       // truncate file
fs.constants.O_APPEND      // append on write
fs.constants.O_DIRECTORY   // fail if not directory
fs.constants.O_NOATIME     // do not update atime
fs.constants.O_NOFOLLOW    // do not follow symlinks
fs.constants.O_SYNC        // synchronous writes
fs.constants.O_DSYNC       // data-only sync
fs.constants.O_DIRECT      // direct I/O (platform dependent)
fs.constants.O_NONBLOCK    // non-blocking mode

// Copy flags (fs.copyFile)
fs.constants.COPYFILE_EXCL           // fail if dest exists
fs.constants.COPYFILE_FICLONE        // copy-on-write reflink
fs.constants.COPYFILE_FICLONE_FORCE  // force reflink

// File type constants (Stats.mode)
fs.constants.S_IFMT        // bitmask for file type
fs.constants.S_IFREG       // regular file
fs.constants.S_IFDIR       // directory
fs.constants.S_IFCHR       // character device
fs.constants.S_IFBLK       // block device
fs.constants.S_IFIFO       // FIFO / pipe
fs.constants.S_IFLNK       // symbolic link
fs.constants.S_IFSOCK      // socket

// File mode bits (POSIX permissions)
fs.constants.S_IRWXU       // owner: read/write/execute
fs.constants.S_IRUSR       // owner: read
fs.constants.S_IWUSR       // owner: write
fs.constants.S_IXUSR       // owner: execute
fs.constants.S_IRWXG       // group: read/write/execute
fs.constants.S_IRGRP       // group: read
fs.constants.S_IWGRP       // group: write
fs.constants.S_IXGRP       // group: execute
fs.constants.S_IRWXO       // others: read/write/execute
fs.constants.S_IROTH       // others: read
fs.constants.S_IWOTH       // others: write
fs.constants.S_IXOTH       // others: execute
```

<details>
<summary>Examples</summary>

```js
fs.accessSync('a.txt', fs.constants.R_OK);                // check read permission
fs.openSync('a.txt', fs.constants.O_RDONLY);              // open read-only
fs.copyFileSync('a.txt', 'b.txt', fs.constants.COPYFILE_EXCL);
const type = fs.statSync('a.txt').mode & fs.constants.S_IFMT;
```

</details>

---

## Notes

* Includes **all callback + sync APIs** (streams included)
* Promise-based APIs live in `node:fs/promises`
* Most methods accept `string | Buffer | URL` paths
