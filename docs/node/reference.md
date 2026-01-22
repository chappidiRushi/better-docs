- [Node JS](#node-js)
  - [what is node js?](#what-is-node-js)
  - [Module System](#module-system)
    - [Using commonJS module system](#using-commonjs-module-system)
    - [Using ESModules system](#using-esmodules-system)
  - [Programming Style](#programming-style)
    - [Non blocking I/O](#non-blocking-io)
    - [Asynchronous Programming](#asynchronous-programming)
    - [Differences](#differences)
    - [Relationship](#relationship)
  - [Console Module](#console-module)
  - [Node.js Process Module](#nodejs-process-module)
  - [OS Module](#os-module)
  - [Path Module](#path-module)
  - [`fs` Module](#fs-module)
  - [`events` Module](#events-module)
    - [notes](#notes)
  - [HTTP Module](#http-module)
    - [Handling Different Request Methods](#handling-different-request-methods)
    - [Handling URL Paths](#handling-url-paths)
  - [Stream Module](#stream-module)
    - [Types of Streams](#types-of-streams)
  - [Node.js Child Processes](#nodejs-child-processes)
    - [Importing the Module and Methods](#importing-the-module-and-methods)
    - [Use Cases](#use-cases)
  - [Node.js Worker Threads](#nodejs-worker-threads)
    - [Methods](#methods)
- [Node.js Parallelism: Comparison Table](#nodejs-parallelism-comparison-table)
  - [Node.js Buffer Cheat Sheet](#nodejs-buffer-cheat-sheet)
  - [🧩 Core Components](#-core-components)
    - [Article](#article)
    - [V8 Engine](#v8-engine)
    - [Libuv](#libuv)
    - [Event Loop](#event-loop)
    - [C++ Bindings](#c-bindings)
    - [Thread Pool](#thread-pool)
  - [⏱️ Event Loop](#️-event-loop)
    - [🔂 Microtasks vs Macrotasks](#-microtasks-vs-macrotasks)
    - [🧠 Task Execution Priority Order (Simplified)](#-task-execution-priority-order-simplified)
    - [🔁 Node.js Task Priority Example](#-nodejs-task-priority-example)
  - [📦  Scaling with Clustering](#--scaling-with-clustering)
    - [🔍 Notes](#-notes)
    - [📌 Tips](#-tips)
  - [Abort Controller](#abort-controller)
  - [🚨 Error Handling](#-error-handling)
    - [🧩 Types of Errors](#-types-of-errors)
    - [🛠️ Combined Error Handling Examples](#️-combined-error-handling-examples)
  - [Interview Questions](#interview-questions)
    - [Beginner (1–30)](#beginner-130)
    - [Intermediate (31–70)](#intermediate-3170)
    - [Advanced / Hardcore (71–100)](#advanced--hardcore-71100)

# Node JS

## what is node js?

- Node is a open source, cross platform JS runtime environment used to build servers.
- Cross platform, performant, easy to start, light weight, efficient , single programming language, Asynchronous & Event driven.

## Module System

We Split code into multiple files for better maintain, organize and reuse.

1. Modules in js nothing but a JS file.
2. Node  supports two types of module systems.
   1. commonJS module(old)
   2. ECMAScript modules(ESM new);

| Feature             | **CommonJS (Node.js Modules)** | **ESM (ECMAScript Modules)**             |
| ------------------- | ------------------------------ | ---------------------------------------- |
| **Syntax**          | `require()`, `module.exports`  | `import`, `export`                       |
| **Loading**         | Synchronous                    | Asynchronous                             |
| **File Extension**  | `.js`                          | `.mjs`, or `.js` with `"type": "module"` |
| **Top-Level Await** | Not supported                  | Supported                                |
| **Browser Support** | Needs bundling/transpiling     | Supported in modern browsers             |

### Using commonJS module system

```javascript
// -----------------EXPORTING ---------------
function add(a, b) { return a + b }
function subtract(a, b) { return a + b }
// exporting using module.exports.
module.exports = { add, subtract }

// export an object
module.exports = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b
}

//  adding functions and props
exports.add = (a,b) => a+b;
exports.myProp = 'hello world'

// or exporting a single function
module.exports = function log(msg) {console.log(msg)};

// -----------------IMPORTING ---------------

// importing whole module
const math = require('./test')
// object destructuring
const {add, subtract} = require('./test')
console.log(math.add(1, 2));
console.log(add(1,2))
```

### Using ESModules system

```javascript
// -----------------EXPORTING ---------------

// re exporting another module.
export { multiply } from './test2.js';
// re exporting everything.
export * from './test2.js';

// Named exports.
export function namedFunction() { console.log('default exp') };
export const namedVar = 'you can export variables too';

// exporting as an object
const myVar = 'const var';
function add(a, b) { return a + b }
export { add, myVar };

// exporting default function
export default function defaultFunc() { console.log('this is default function'); }

// -----------------IMPORTING ---------------
// import everything as an object.
import * as Utils from './test.js'

// import Named exports.
import { add, multiply } from "./test.js";

// import with Named alias.
import { multiply as UtilMultiply } from "./test2.js";

// import default
import MathUtil from "./test.js";

// import default and Named.
import MathUtil, { add, multiply } from "./test.js";

// Dynamic imports.
import('./test.js').then((module) => { console.log(module.add(1, 2)); })

// Top level await(ESM Only)
const MathModule = await import('./test.js')
console.log(MathModule.add(1,2));

```

## Programming Style

### Non blocking I/O

Non-blocking programming is a technique used to improve the responsiveness of an application by allowing it to perform multiple I/O operations simultaneously without blocking. In non-blocking programming, the application doesn’t wait for the completion of a task before moving on to the next task. Instead, **it checks the status of each task periodically** and only acts on completed tasks.

### Asynchronous Programming

Asynchronous programming is another technique used to improve the performance of an application by allowing it to perform multiple tasks simultaneously. In asynchronous programming, the application doesn’t wait for the completion of a task before moving on to the next task. However, unlike non-blocking programming,**the application doesn’t have to periodically check the status of each task. Instead, it can continue executing other code until the task is complete, and then the application is notified of the completion.**

### [Differences](https://medium.com/@livajorge7/breaking-down-the-differences-non-blocking-vs-asynchronous-programming-and-when-to-use-each-953f2354b052#:~:text=Non%2Dblocking%20operations%20refer%20to,concurrently%20without%20blocking%20other%20operations.)

| Feature             | Non-Blocking I/O                                   | Asynchronous Programming                                                                                      |
| ------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Focus**           | How I/O operations are handled at the OS level.    | How code is structured to handle delayed results.                                                             |
| **Level**           | Operating system and Node.js core.                 | Application-level code.                                                                                       |
| **Mechanism**       | OS handles I/O without blocking the main thread.   | Callbacks, Promises, Async/Await.                                                                             |
| **Purpose**         | Improve system responsiveness and efficiency.      | Improve code readability, maintainability, and handle concurrency.                                            |
| **Example**         | Reading a file from disk, network requests.        | Handling the result of a file read or network request.                                                        |
| **Implementation**  | OS-level mechanisms (e.g., epoll, kqueue, IOCP).   | JavaScript language features and Node.js APIs.                                                                |
| **Relationship**    | Non-blocking I/O enables asynchronous programming. | Asynchronous programming utilizes non-blocking I/O.                                                           |
| **Blocking**        | I/O operations do not block the main thread.       | Code execution can appear to be blocking (e.g., when using `await`), but the event loop is not blocked.       |
| **Concurrency**     | Enables concurrent execution of I/O operations.    | Allows for concurrent execution of code and I/O operations.                                                   |
| **Event Loop Role** | Relies heavily on the event loop to manage I/O.    | Uses the event loop to schedule and execute callbacks, promise resolutions, and async function continuations. |
| **Abstraction**     | Lower-level abstraction.                           | Higher-level abstraction.                                                                                     |

### Relationship

1. Non-blocking I/O is the foundation upon which asynchronous programming is built.
2. Node.js leverages non-blocking I/O to enable efficient asynchronous programming.
3. In short, the OS does the Non-blocking I/O, and the javascript code uses asynchronous programming to react to the results of those I/O operations.

## Console Module

```javascript
console.log("This is a log message");
console.info("This is an info message");
console.warn("This is a warning message");
console.error("This is an error message");
console.log("Formatted string: %s, Number: %d, JSON: %j", "hello", 42, { key: "value" });
console.debug("This is a debug message");

console.time("Timer");
setTimeout(() => {
  console.timeLog("Timer");
  console.timeEnd("Timer");
}, 1000);

console.count("Counter");
console.count("Counter");
console.count("Counter");
console.countReset("Counter");
console.count("Counter");

console.table([
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 }
]);

console.group("Group Start");
console.log("Inside the group");
console.groupCollapsed("Collapsed Group");
console.log("Inside the collapsed group");
console.groupEnd();
console.trace("Stack Trace Example");
console.clear(); // Clears the console (may not work in some environments)

```

## Node.js Process Module

- The `process` module allows interaction with the Node.js process.
- Provides system info, command-line arguments, and environment variables.
- Allows managing process lifecycle and events.

```js
// Accessing process module.
console.log(process);

// Important Properties.
// Returns an array of command-line arguments.
console.log(process.argv);
// Provides access to environment variables.
console.log(process.env);

// Returns the process ID.
console.log(`Process ID: ${process.pid}`);
-
// Returns the current working directory.
console.log(process.cwd());

// Returns the operating system platform.
console.log(process.platform);

// Important Methods
// Exits the process with the given exit code (default: `0`).
process.exit(1);
// Handles process events like `exit`, `uncaughtException`, etc.
process.on('exit', (code) => {
    console.log(`Process exited with code: ${code}`);
});
// Returns the uptime of the process in seconds.
console.log(`Uptime: ${process.uptime()} seconds`);
// Returns memory usage statistics.
console.log(process.memoryUsage());
// Defers the execution of a function until the next event loop iteration.
process.nextTick(() => {console.log("Executed in next tick");});

// Common events.
// other optios: 'uncaughtException', 'SIGINT'
process.on('exit', () => {
    console.log('Process is exiting...');
});
```

## OS Module

The `os` module helps gather system-related information such as CPU details, memory usage, and network interfaces. It is useful for system monitoring and resource management in Node.js applications.

```js
const os = require('os');

//  Methods and Properties
console.log(os.arch()); //operating system CPU architecture e.g., 'x64'
console.log(os.platform()); // e.g.,OS platform 'win32', 'linux', 'darwin'
console.log(os.type()); // operating system name. 'Windows_NT'
console.log(os.release()); // OS release e.g., '10.0.19041'
console.log(os.totalmem());// total system memory in bytes.
console.log(os.freemem()); // free system memory in bytes
// Returns an array of objects containing information about each logical CPU core.
console.log(os.cpus());
console.log(os.homedir());// home directory of the current user
console.log(os.hostname());//hostname of the OS
console.log(os.networkInterfaces());// Information about network interfaces
console.log(os.uptime()); //system uptime in seconds
// Example
const os = require('os');
console.log(`System: ${os.type()} ${os.release()}`);
console.log(`Architecture: ${os.arch()}`);
console.log(`Total Memory: ${os.totalmem() / (1024 * 1024 * 1024)} GB`);
console.log(`Free Memory: ${os.freemem() / (1024 * 1024 * 1024)} GB`);
console.log(`CPU Cores: ${os.cpus().length}`);
console.log(`Home Directory: ${os.homedir()}`);
console.log(`Uptime: ${os.uptime()} seconds`);

```

## Path Module

The path module provides utilities for working with file and directory paths. It allows manipulation of paths in a cross-platform manner, handling different path formats for Windows and POSIX systems.

```js
const path = require('path');

// ---------------------------
// Basic Path Info
// ---------------------------
const filePath = '/user/home/index.html';

console.log('Directory:', path.dirname(filePath)); // '/user/home'
console.log('Base Name:', path.basename(filePath)); // 'index.html'
console.log('Extension:', path.extname(filePath)); // '.html'

// ---------------------------
// Join & Resolve Paths
// ---------------------------
console.log(path.join('/user', 'home', '../', 'index.html')); // '/user/index.html'
console.log(path.resolve('index.html')); // Absolute path to 'index.html'

// Difference between join and resolve
console.log(path.join('/foo', '/bar'));    // '/foo/bar'
console.log(path.resolve('/foo', '/bar')); // '/bar'

// ---------------------------
// Path Checks
// ---------------------------
console.log(path.isAbsolute('/user/home'));       // true
console.log(path.isAbsolute('home/index.html'));  // false

// ---------------------------
// Normalize Paths
// ---------------------------
console.log(path.normalize('/user/home/foo/..')); // '/user/home'

// ---------------------------
// Parse & Format Paths
// ---------------------------
console.log(path.parse(filePath));
/*
{
  root: '/',
  dir: '/user/home',
  base: 'index.html',
  ext: '.html',
  name: 'index'
}
*/

console.log(path.format({ dir: '/user/home', base: 'index.html' })); // '/user/home/index.html'

// ---------------------------
// Relative Paths
// ---------------------------
console.log(path.relative('/user/home', '/user/home/index.html')); // 'index.html'
console.log(path.relative('/foo/bar', '/foo/baz/file.txt'));      // '../baz/file.txt'

// ---------------------------
// OS-specific Utilities
// ---------------------------
console.log('Path Separator:', path.sep);       // '/' on POSIX, '\' on Windows
console.log('Path Delimiter:', path.delimiter); // ':' on POSIX, ';' on Windows

// POSIX-style path
console.log(path.posix.join('/user', 'home', 'index.html')); // '/user/home/index.html'

// Windows-style path (use double backslashes!)
console.log(path.win32.join('C:\\user', 'home', 'index.html')); // 'C:\user\home\index.html'

// ---------------------------
// Edge Cases
// ---------------------------
console.log(path.extname('archive.tar.gz')); // '.gz'
console.log(path.basename('archive.tar.gz')); // 'archive.tar.gz'

// Parsing root path
console.log(path.parse('/'));
/*
{
  root: '/',
  dir: '/',
  base: '',
  ext: '',
  name: ''
}
*/

```

## `fs` Module

The `fs` module provides functions to read, write, delete, and manage files and directories in Node.js applications.

```js
const path = require('path');

// === Path Separators and Delimiters ===
path.sep;           // OS-specific path segment separator ('/' or '\\')
path.delimiter;     // OS-specific PATH delimiter (':' or ';')

// === Path Operations ===
path.basename('/dir/file.txt');                  // 'file.txt'
path.basename('/dir/file.txt', '.txt');          // 'file'

path.dirname('/dir/file.txt');                   // '/dir'

path.extname('/dir/file.txt');                   // '.txt'

path.format({
  dir: '/dir/sub',
  base: 'file.txt'
}); // '/dir/sub/file.txt'

path.parse('/dir/sub/file.txt');
/*
{
  root: '/',
  dir: '/dir/sub',
  base: 'file.txt',
  ext: '.txt',
  name: 'file'
}
*/

path.isAbsolute('/foo');                         // true
path.isAbsolute('foo');                          // false

path.join('foo', 'bar', 'baz/asdf');             // 'foo/bar/baz/asdf'

path.normalize('/foo/bar//baz/asdf/..');         // '/foo/bar/baz'

path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
  // '../../impl/bbb'

path.resolve('foo/bar', '/tmp/file/', '..', 'a.js');
  // resolves to absolute path like '/tmp/a.js'

// === Platform-Specific Implementations ===
path.posix;  // POSIX-specific methods (e.g., path.posix.join())
path.win32;  // Windows-specific methods (e.g., path.win32.join())

// Example: always use POSIX-style separators
path.posix.join('foo', 'bar'); // 'foo/bar'

// === Edge Cases ===
path.basename('');             // '' - empty input
path.extname('index');         // '' - no extension
path.extname('.index');        // '' - dotfile with no ext

// === Notes ===
// All methods are synchronous and do not access the file system
// Use `path.resolve()` when constructing absolute paths
// Use `path.join()` when creating relative paths

```

## `events` Module

The `events` module provides the **EventEmitter** class, which allows objects to **emit named events** and **subscribe to them** with listener functions. Many Node.js core modules (like `fs`, `http`, and `net`) use EventEmitter internally.

```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

// === Adding Listeners ===
emitter.on('event', () => { console.log('Event triggered'); });
emitter.addListener('event', () => { console.log('Another listener'); }); // alias for `on`
emitter.once('init', () => { console.log('Initialization done!'); }); // runs once
emitter.prependListener('event', () => { console.log('Runs first'); }); // runs before other listeners
emitter.prependOnceListener('start', () => { console.log('Runs first, once'); });

// === Removing Listeners ===
const callback = (data) => console.log('Data:', data);
emitter.on('data', callback);
emitter.off('data', callback);                // removes listener
emitter.removeListener('data', callback);     // older alias
emitter.removeAllListeners('event');          // removes all listeners for a specific event
emitter.removeAllListeners();                 // removes all listeners for all events

// === Emitting Events ===
emitter.emit('event', 'Hello');               // triggers all listeners, returns true if any existed
emitter.emit('init');                          // triggers once-listener
emitter.emit('nonexistent');                  // returns false, no listeners

// === Inspecting Listeners ===
emitter.listeners('event');                   // array of listeners
emitter.rawListeners('event');                // original listeners including wrappers for `once`
emitter.listenerCount('event');               // number of listeners
emitter.eventNames();                         // array of event names with registered listeners

// === Managing Max Listeners ===
emitter.setMaxListeners(20);                  // set limit for this instance
emitter.getMaxListeners();                    // get current limit
EventEmitter.defaultMaxListeners;             // default limit (10)

// === Common Patterns ===
// Event-driven class example
class MyServer extends EventEmitter {
  start() {
    console.log('Server starting...');
    this.emit('start');
  }
}
const server = new MyServer();
server.on('start', () => console.log('Server has started'));
server.start();

// === Edge Cases ===
emitter.on('event', null);                   // TypeError: listener must be a function
emitter.emit('nonexistent');                 // false, no listeners

```

### notes

- `on` for persistent listeners, `once` for single-use
- `prependListener` ensures a listener runs before others
- `rawListeners` returns the original functions (useful for `once`)
- `setMaxListeners()` prevents memory leak warnings
- `emit()` calls listeners synchronously

## HTTP Module

The `http` module in Node.js allows you to create an HTTP server and handle requests and responses.

- The `http` module enables creating web servers.
- `createServer` is used to handle requests and responses.
- You can differentiate requests using `req.method` and `req.url`.
- The `http.request` method is used for making API calls.

```javascript

const http = require('http');

// === Create a Basic HTTP Server ===
const server = http.createServer((req, res) => {
  res.statusCode = 200; // default status
  res.setHeader('Content-Type', 'text/plain'); // set response header
  res.end('Hello World\n'); // end response
});

server.listen(3000, 'localhost', () => {
  console.log('Server running at http://localhost:3000/');
});

// === Make an HTTP GET Request ===
http.get('http://example.com', (res) => {
  console.log(`Status: ${res.statusCode}`);
  res.setEncoding('utf8'); // optional: decode response data
  res.on('data', (chunk) => {
    console.log(`BODY: ${chunk}`);
  });
  res.on('end', () => {
    console.log('No more data.');
  });
}).on('error', (e) => {
  console.error(`Got error: ${e.message}`);
});

// === Make a Custom HTTP Request ===
const options = {
  hostname: 'example.com',
  port: 80,
  path: '/',
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  }
};

const req = http.request(options, (res) => {
  console.log(`STATUS: ${res.statusCode}`);
  res.on('data', (chunk) => {
    console.log(`BODY: ${chunk}`);
  });
});

req.on('error', (e) => {
  console.error(`problem with request: ${e.message}`);
});

req.write(JSON.stringify({ msg: 'Hello' })); // send body data
req.end(); // finalize request

// === Server Events ===
server.on('request', (req, res) => {});       // when a request is received
server.on('connection', (socket) => {});      // when a new TCP connection is made
server.on('close', () => {});                 // when server is closed
server.on('error', (err) => {});              // on error

// === Response Object Methods ===
res.writeHead(200, { 'Content-Type': 'text/plain' }); // set status + headers
res.write('Hello'); // write chunk
res.end('World');   // end response

// === Request Object ===
req.method     // e.g., 'GET', 'POST'
req.url        // request URL path
req.headers    // object with request headers
req.on('data', chunk => {}); // receive request body
req.on('end', () => {});     // finished receiving data

// === HTTP Constants ===
http.STATUS_CODES[200];      // 'OK'
http.METHODS.includes('POST'); // true

// === Notes ===
// - For HTTPS use: require('https')
// - `http` is suitable for lightweight use; use `express` for higher-level APIs
```

### Handling Different Request Methods

```javascript
const server = http.createServer((req, res) => {
    if (req.method === 'GET') {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('This is a GET request');
    } else if (req.method === 'POST') {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('This is a POST request');
    } else {
        res.writeHead(405, { 'Content-Type': 'text/plain' });
        res.end('Method Not Allowed');
    }
});
server.listen(3000);
```

### Handling URL Paths

```javascript
const server = http.createServer((req, res) => {
    if (req.url === '/') {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('Welcome to the homepage');
    } else if (req.url === '/about') {
        res.writeHead(200, { 'Content-Type': 'text/plain' });
        res.end('About us page');
    } else {
        res.writeHead(404, { 'Content-Type': 'text/plain' });
        res.end('Page not found');
    }
});
server.listen(3000);
```

## Stream Module

Node.js streams are used for handling data in chunks, making them efficient for reading or writing large files, network communications, or processing data progressively.

- Streams help handle large amounts of data efficiently.
- Use `pipe()` to pass data between streams.
- Always handle errors in streams.
- Transform streams modify data while transferring it.

### Types of Streams

1. **Readable Streams** - Used for reading data (e.g., `fs.createReadStream`).
2. **Writable Streams** - Used for writing data (e.g., `fs.createWriteStream`).
3. **Duplex Streams** - Both readable and writable (e.g., `net.Socket`).
4. **Transform Streams** - A special type of Duplex stream that can modify data while reading and writing (e.g., `zlib.createGzip`).
5.

```javascript
const fs = require('fs');
const { Transform, PassThrough, pipeline, finished } = require('stream');

// === Readable Stream ===
const readableStream = fs.createReadStream('file.txt', { encoding: 'utf8' });
// Methods: on, read, pause, resume, pipe, unpipe, isPaused, destroy, unshift, wrap
// Events: close, data, end, error, open, pause, readable, read, resume

readableStream.on('data', (chunk) => {
    console.log('Received chunk:', chunk);
});
readableStream.on('end', () => {
    console.log('Finished reading.');
});
readableStream.on('error', (err) => {
    console.error('Read error:', err.message);
});

// === Writable Stream ===
const writableStream = fs.createWriteStream('output.txt');
// Methods: write, end, cork, uncork, setDefaultEncoding, destroy, on, once, emit, pipe
// Events: close, drain, error, finish, pipe, unpipe

writableStream.write('Hello, world!\n');
writableStream.end();
writableStream.on('finish', () => {
    console.log('Write complete.');
});
writableStream.on('error', (err) => {
    console.error('Write error:', err.message);
});

// === Duplex Stream (read + write) ===
const net = require('net');
const server = net.createServer((socket) => {
    // socket is a Duplex stream
    socket.write('Echo server\r\n');
    socket.pipe(socket); // echoes input back
});
server.listen(1337);

// === Transform Stream ===
const toUpperCase = new Transform({
    transform(chunk, encoding, callback) {
        this.push(chunk.toString().toUpperCase());
        callback();
    }
});

process.stdin.pipe(toUpperCase).pipe(process.stdout);

// === PassThrough Stream (acts as a middleman) ===
const pass = new PassThrough();
pass.on('data', chunk => console.log('PassThrough:', chunk.toString()));
pass.write('test\n');
pass.end();

// === Stream Piping ===
const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt');
readStream.pipe(writeStream);

// === Stream.pipeline (preferred for robust error handling) ===
pipeline(
    fs.createReadStream('input.txt'),
    new Transform({
        transform(chunk, enc, cb) {
            this.push(chunk.toString().replace(/foo/g, 'bar'));
            cb();
        }
    }),
    fs.createWriteStream('processed.txt'),
    (err) => {
        if (err) console.error('Pipeline failed:', err);
        else console.log('Pipeline succeeded');
    }
);

// === Stream.finished (detect when stream ends/errors) ===
finished(readStream, (err) => {
    if (err) console.error('Stream failed:', err.message);
    else console.log('Stream ended.');
});

```

Streams are powerful tools in Node.js for handling data efficiently and asynchronously!

## Node.js Child Processes

Node.js provides the `child_process` module to spawn child processes and execute shell commands. This allows running external programs, handling system tasks, and executing scripts in parallel.

### Importing the Module and Methods

```js
const { exec, execFile, spawn, fork } = require('child_process');

// === exec(): shell command with stdout/stderr ===
exec('ls -la', (error, stdout, stderr) => {
  if (error) {
    console.error(`Exec error: ${error.message}`);
    return;
  }
  if (stderr) {
    console.error(`Stderr: ${stderr}`);
    return;
  }
  console.log(`Exec stdout:\n${stdout}`);
});

// === execFile(): run a binary without a shell ===
execFile('node', ['hello.js'], (error, stdout, stderr) => {
  if (error) {
    console.error(`ExecFile error: ${error.message}`);
    return;
  }
  console.log(`ExecFile stdout: ${stdout}`);
});

// === spawn(): for streaming data, long-running processes ===
const { spawn } = require('child_process');

// === Spawn a child process ===
const child = spawn('ls', ['-lh', '/usr']);

// === Handle standard output (stdout) ===
child.stdout.on('data', (data) => {
  console.log(`Spawn stdout: ${data}`);
});

// === Handle standard error (stderr) ===
child.stderr.on('data', (data) => {
  console.error(`Spawn stderr: ${data}`);
});

// === Handle process exit ===
child.on('close', (code) => {
  console.log(`Spawn child exited with code ${code}`);
});

// === Handle spawn errors ===
child.on('error', (error) => {
  console.error(`Spawn error: ${error.message}`);
});

// === Handle exit event separately (gives code and signal) ===
child.on('exit', (code, signal) => {
  console.log(`Child exited with code ${code}, signal ${signal}`);
});

// === Write to child stdin ===
child.stdin.write('Some input\n');

// === Close stdin when done ===
child.stdin.end();

// === Kill the child process (optional) ===
child.kill('SIGTERM'); // default signal is SIGTERM

// === Unreference the child (parent can exit independently) ===
child.unref();

// === Reference again if needed ===
child.ref();

// === Notes on other events (for forked processes) ===
// 'disconnect' -> emitted if parent calls child.disconnect()
// 'message' -> emitted when child sends message using process.send()

// === spawn with options ===
const ps = spawn('ps', ['aux'], {
  stdio: 'inherit', // direct stream to parent
  shell: true       // run in shell
});

// === fork(): for Node.js module communication ===
const forkedChild = fork('childScript.js');

forkedChild.on('message', (message) => {
  console.log(`Message from child: ${message}`);
});

forkedChild.send({ task: 'do something' });

forkedChild.on('exit', (code) => {
  console.log(`Forked child exited with code ${code}`);
});

// childScript.js
// Listen for messages from the parent
process.on('message', (msg) => {
  console.log('Child received:', msg);

  // Send a message back
  process.send({ reply: 'Hello Parent!' });
});

// Optional: exit after a delay
setTimeout(() => {
  process.exit(0);
}, 1000);


// === Additional: kill a child process ===
setTimeout(() => {
  child.kill(); // send SIGTERM by default
  console.log('Child process killed');
}, 5000);

// === Check if child is still running ===
if (!child.killed) {
  console.log('Child is running...');
}

```

### Use Cases

- Running shell commands
- Automating scripts
- Parallel processing
- Managing background tasks

## Node.js Worker Threads

Worker Threads in Node.js allow you to run JavaScript code in parallel for CPU-intensive tasks.

```js
// === main-basic.mjs ===
import { Worker } from 'worker_threads';
const worker = new Worker(new URL('./worker-basic.mjs', import.meta.url), {
  workerData: { value: 10 }
});

worker.on('message', (msg) => {console.log('Main received:', msg);});

worker.on('error', (err) => {console.error('Worker error:', err);});

worker.on('exit', (code) => {  console.log('Worker exited with code:', code);});

worker.postMessage('Compute square');

setTimeout(() => {
  worker.terminate().then(code => {
    console.log('Worker terminated with code:', code);
  });
}, 3000);


// === worker-basic.mjs ===
import { parentPort, workerData } from 'worker_threads';

console.log('Worker started with data:', workerData);

parentPort.on('message', (msg) => {
  if (msg === 'Compute square') {
    const result = workerData.value ** 2;
    parentPort.postMessage(`Squared result: ${result}`);
  }
});

// === main-port.mjs ===
import { Worker, MessageChannel } from 'worker_threads';

const worker = new Worker(new URL('./worker-port.mjs', import.meta.url));
const { port1, port2 } = new MessageChannel();

port1.on('message', (msg) => {  console.log('Main received via port:', msg);});
worker.postMessage({ port: port2 }, [port2]);

// === worker-port.mjs ===
import { parentPort } from 'worker_threads';

parentPort.on('message', (msg) => {
  if (msg.port) {
    const port = msg.port;
    port.postMessage('Hello from worker via MessagePort');
    port.on('message', (data) => {
      console.log('Worker received via port:', data);
    });
  }
});
// === main-shared.mjs ===
import { Worker } from 'worker_threads';

const sharedBuffer = new SharedArrayBuffer(4); // 4 bytes = 1 Int32
const sharedArray = new Int32Array(sharedBuffer);
sharedArray[0] = 21;

const worker = new Worker(new URL('./worker-shared.mjs', import.meta.url));
worker.on('message', (msg) => {
  console.log('Main received:', msg);
  console.log('Updated shared value:', sharedArray[0]);
});

worker.postMessage({ sharedBuffer });

// === worker-shared.mjs ===
import { parentPort } from 'worker_threads';

parentPort.on('message', (msg) => {
  if (msg.sharedBuffer) {
    const arr = new Int32Array(msg.sharedBuffer);
    console.log('Worker read shared value:', arr[0]);
    arr[0] = arr[0] * 2;
    parentPort.postMessage(`Shared buffer updated to ${arr[0]}`);
  }
});

```

### Methods

- `new Worker(filename, options)`: Creates a new worker thread.
- `worker.on('message', callback)`: Listens for messages from the worker.
- `worker.postMessage(data)`: Sends a message to the worker.
- `worker.on('error', callback)`: Handles errors in the worker.
- `worker.terminate()`: Stops the worker thread.

# Node.js Parallelism: Comparison Table

| Feature                | Worker Thread                                    | Fork (Node Process)                     | spawn/exec (Child Process)               |
| ---------------------- | ------------------------------------------------ | --------------------------------------- | ---------------------------------------- |
| Runs JavaScript code?  | ✅ Yes                                            | ✅ Yes                                   | ❌ No (any command)                       |
| Separate memory space? | ❌ Shares memory via SharedArrayBuffer (optional) | ✅ Each fork has own memory              | ✅ Each process has own memory            |
| Separate event loop?   | ✅                                                | ✅                                       | ✅                                        |
| Lightweight?           | ✅ Low overhead                                   | ⚡ Medium overhead                       | ❌ Heavy (full OS process)                |
| Communication          | Message + Shared Memory                          | Message (`.send()`)                     | stdout/stderr streams                    |
| Best for               | CPU-heavy JS tasks, parallel computation         | Heavy Node tasks, isolation, crash-safe | Running shell commands or other programs |
| Crash isolation        | ❌ Crashes main thread                            | ✅ Crashes child only                    | ✅ Crashes child only                     |
| Example                | `new Worker(...)`                                | `fork('child.js')`                      | `spawn('ls', ['-lh'])`                   |

## Node.js Buffer Cheat Sheet

- Buffers handle binary data directly.
- Fixed-size raw memory allocations.
- Useful for I/O (files, TCP streams).

```js
// Creating Buffers
// Deprecated (avoid):
Buffer(size)         // Uninitialized buffer
Buffer(string)       // From string

// Recommended:
Buffer.alloc(size)                // Zero-filled buffer
Buffer.allocUnsafe(size)          // Faster, uninitialized (unsafe)
Buffer.from(array)                // From array of bytes
Buffer.from(string, [encoding])  // From string, available encoding types=  'utf8' (default), 'ascii','base64','hex','latin1','utf16le','ucs2'

// ## Writing to Buffer
buf.write(string, offset=0, length=buf.length - offset, encoding='utf8')
// Returns bytes written

// ## Reading from Buffer
buf.toString([encoding='utf8'], [start=0], [end=buf.length])
buf.readUInt8(offset)
buf.readUInt16LE(offset)
buf.readUInt16BE(offset)
buf.readUInt32LE(offset)
buf.readUInt32BE(offset)
buf.readInt8(offset)
buf.readInt16LE(offset)
buf.readInt16BE(offset)
buf.readInt32LE(offset)
buf.readInt32BE(offset)

// ## Buffer Length & Size
buf.length  // Number of bytes
//  Copying Buffers
buf.copy(targetBuffer, targetStart=0, sourceStart=0, sourceEnd=buf.length)

//  Slicing Buffers
const slice = buf.slice(start, end)  // Shares memory (view)

//  Buffer Comparison
Buffer.compare(buf1, buf2)  // Returns 0, 1, or -1
buf1.equals(buf2)           // true if identical

//  Filling Buffer
buf.fill(value, [offset=0], [end=buf.length], [encoding='utf8'])

//  Concatenating Buffers
Buffer.concat([buf1, buf2, ...], totalLength)

//  Checking Buffer
Buffer.isBuffer(obj)  // true if obj is a Buffer

//  Other Useful Methods
buf.includes(value, byteOffset=0, encoding)
buf.indexOf(value, byteOffset=0, encoding)
buf.lastIndexOf(value, byteOffset=buf.length-1, encoding)

//  Example Usage
const buf = Buffer.from('hello', 'utf8')
console.log(buf.toString())  // 'hello'

const buf2 = Buffer.alloc(5)
buf2.write('world')
console.log(buf2.toString())  // 'world'

const combined = Buffer.concat([buf, buf2])
console.log(combined.toString())  // 'helloworld'

//  Notes
- Buffers are fixed-size (not resizable).
- Buffer extends Uint8Array.
- Always specify encoding when converting string <-> buffer.
```

## 🧩 Core Components

### [Article](https://www.builder.io/blog/visual-guide-to-nodejs-event-loop)

### V8 Engine

- JavaScript engine from Chrome
- Compiles JS to machine code
- Handles memory management and GC

### Libuv

- A C library for cross-platform asynchronous I/O
- Provides:
  - **Event Loop**
  - **Thread Pool**
  - **Non-blocking I/O abstractions**

### Event Loop

- Orchestrates the execution of asynchronous operations
- Divided into multiple **phases** (explained below)

### C++ Bindings

- Bridges Node.js JavaScript APIs with native OS capabilities
- Example: `fs.readFile()` calls a native binding, which uses Libuv

### Thread Pool

- Used by Libuv for I/O-heavy operations like:
  - File system
  - DNS
  - zlib compression
  - crypto

## ⏱️ Event Loop

```
┌────────────────────────────┐
│ 1. timers                  │ → `setTimeout`, `setInterval`
│ 2. pending callbacks       │ → I/O callbacks from previous loop
│ 3. idle, prepare           │ → Internal use
│ 4. poll                    │ → New I/O events; may block
│ 5. check                   │ → `setImmediate`
│ 6. close callbacks         │ → Cleanup events (e.g. `socket.on('close')`)
└────────────────────────────┘
```

---

### 🔂 Microtasks vs Macrotasks

| Type          | Queue Name         | Priority  | Examples                            |
| ------------- | ------------------ | --------- | ----------------------------------- |
| **Microtask** | Microtask Queue    | 🟠 Medium  | `Promise.then`, `queueMicrotask()`  |
| **Microtask** | Next Tick Queue    | 🔴 Highest | `process.nextTick()` (Node.js only) |
| **Macrotask** | Timer Queue        | 🟡 Normal  | `setTimeout`, `setInterval`         |
| **Macrotask** | Check Queue        | 🟢 Normal  | `setImmediate()`                    |
| **Macrotask** | I/O Callback Queue | 🟢 Normal  | `fs`, `net`, `dns`, etc.            |

---

### 🧠 Task Execution Priority Order (Simplified)

1. **process.nextTick()** – Executes **before any promise**
2. **Microtasks** – `Promise.then`, `queueMicrotask()`
3. **Macrotasks** – `setTimeout`, `setImmediate`, I/O callbacks

---

### 🔁 Node.js Task Priority Example

This example shows the order in which different types of tasks are executed in Node.js.

```js
console.log('start');

process.nextTick(() => console.log('nextTick 1'));

Promise.resolve().then(() => console.log('promise 1'));

setTimeout(() => console.log('timeout 1'), 0);

setImmediate(() => console.log('immediate 1'));

process.nextTick(() => {
  console.log('nextTick 2');
  Promise.resolve().then(() => console.log('promise 2'));
});

console.log('end');
start
// end
// nextTick 1
// nextTick 2
// promise 1
// promise 2
// timeout 1
// immediate 1
```

## 📦  Scaling with Clustering

Node.js runs in a single-threaded event loop model. To utilize multi-core systems and improve scalability, Node.js provides the **Cluster module**, which allows you to create child processes (workers) that share the same server port.

- Leverage **multi-core CPUs**.
- Handle **more concurrent requests**.
- Improve **fault tolerance**.
- Isolate **crashing processes**.

```
          ┌────────────┐
          │   Master   │
          └────┬───────┘
               │
     ┌─────────┼─────────┐
     ▼         ▼         ▼
 ┌──────┐  ┌──────┐  ┌──────┐
 │Worker│  │Worker│  │Worker│
 └──────┘  └──────┘  └──────┘
```

- **Master**: Manages worker lifecycle.
- **Workers**: Handle actual HTTP requests.

```js
// cluster-simple.js
import cluster from 'cluster';
import http from 'http';
import os from 'os';

const numCPUs = os.cpus().length;

if (cluster.isPrimary) {
  // Master process
  console.log(`Master ${process.pid} is running`);

  // Set Round Robin scheduling
  cluster.schedulingPolicy = cluster.SCHED_RR;

  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    const worker = cluster.fork(); // fork creates a new worker process

    // Worker events

    // Fired when worker is online
    worker.on('online', () => {
      console.log(`Master: Worker ${worker.process.pid} is online`);
    });

    // Fired when worker sends a message
    worker.on('message', (msg) => {
      console.log(`Master: Received message from worker ${worker.process.pid}:`, msg);
    });

    // Fired when worker disconnects from master
    worker.on('disconnect', () => {
      console.log(`Master: Worker ${worker.process.pid} disconnected`);
    });

    // Fired when worker exits (crashes or killed)
    worker.on('exit', (code, signal) => {
      console.log(`Master: Worker ${worker.process.pid} exited (code=${code}, signal=${signal})`);
      // Optional: restart worker
      cluster.fork();
    });

    // Access worker properties
    console.log(`Master: Worker ID=${worker.id}, PID=${worker.process.pid}`);
    console.log(`Master: isConnected()=${worker.isConnected()}, isDead()=${worker.isDead()}`);

    // Send message to worker
    worker.send({ msg: `Hello Worker ${worker.id}` });

    // Example: gracefully disconnect first worker after 10s
    if (worker.id === 1) {
      setTimeout(() => {
        console.log(`Master: Disconnecting worker ${worker.process.pid}`);
        worker.disconnect(); // finishes current connections before disconnecting
      }, 10000);
    }

    // Example: forcefully kill second worker after 15s
    if (worker.id === 2) {
      setTimeout(() => {
        console.log(`Master: Killing worker ${worker.process.pid}`);
        worker.kill(); // sends SIGTERM
      }, 15000);
    }
  }
} else {
  // Worker process
  console.log(`Worker ${process.pid} started`);

  // Access worker methods/properties
  console.log(`Worker: ID=${cluster.worker.id}`);
  console.log(`Worker: isDead()=${cluster.worker.isDead()}`);
  console.log(`Worker: process.pid=${process.pid}`);
  console.log(`Worker: process.connected=${process.connected}`);

  // Create HTTP server
  const server = http.createServer((req, res) => {
    res.writeHead(200);
    res.end(`Hello from worker ${process.pid}\n`);
  });

  server.listen(3000, () => {
    console.log(`Worker ${process.pid} listening on port 3000`);
  });

  // Receive messages from master
  process.on('message', (msg) => {
    console.log(`Worker ${process.pid} received:`, msg);
    // Reply back to master
    process.send({ pid: process.pid, reply: 'Message received!' });
  });

  // Periodically send status to master
  setInterval(() => {
    process.send({
      pid: process.pid,
      connected: process.connected,
      isDead: cluster.worker.isDead(),
    });
  }, 5000);

  // Handle graceful disconnect from master
  process.on('disconnect', () => {
    console.log(`Worker ${process.pid} disconnect event fired`);
    // Close server before exiting
    server.close(() => {
      console.log(`Worker ${process.pid} exiting gracefully`);
      process.exit(0);
    });
  });
}

```

### 🔍 Notes

- Workers share the **same port**.
- Each worker is a **separate Node.js process**.
- Ideal for **stateless** workloads.

### 📌 Tips

- Use a **load balancer** in production (e.g., Nginx).
- Store state externally (Redis, DB) to avoid memory duplication.
- Combine with **PM2** or **Docker** for process management.

## Abort Controller

```js
/**
 * AbortController – Simple, Complete Example
 * Demonstrates:
 * - AbortController()
 * - controller.abort(reason)
 * - signal.aborted
 * - signal.reason
 * - signal.addEventListener()
 * - signal.removeEventListener()
 * - signal.throwIfAborted()
 */

const controller = new AbortController();
const signal = controller.signal;

// Listener function (so we can remove it later)
function onAbort() {
  console.log("❌ Abort event fired");
  console.log("Abort reason:", signal.reason);
}

// Listen for abort
signal.addEventListener("abort", onAbort);

// Example async function that supports aborting
async function doWork(signal) {
  console.log("▶ Work started");

  // Throws immediately if already aborted
  signal.throwIfAborted();

  return new Promise((resolve, reject) => {
    const timeout = setTimeout(() => {
      console.log("✅ Work finished");
      resolve("SUCCESS");
    }, 3000);

    signal.addEventListener(
      "abort",
      () => {
        clearTimeout(timeout); // cleanup
        reject(signal.reason); // use abort reason
      },
      { once: true }
    );
  });
}

// Abort after 1 second
setTimeout(() => {
  console.log("⚠ Calling abort()");
  controller.abort(new Error("Operation cancelled by user"));
}, 1000);

// Run
(async () => {
  try {
    const result = await doWork(signal);
    console.log("Result:", result);
  } catch (err) {
    console.log("Caught error:", err.message);
  } finally {
    console.log("Signal aborted?", signal.aborted);

    // Remove listener (good practice)
    signal.removeEventListener("abort", onAbort);

    console.log("Done.");
  }
})();

```

## 🚨 Error Handling

Proper error handling is critical in Node.js to create robust, fault-tolerant applications.

### 🧩 Types of Errors

- **Synchronous Errors:** Thrown immediately, can be caught using `try...catch`.
- **Asynchronous Errors:** Occur in callbacks, promises, or event handlers.
- **Operational Errors:** Runtime problems like network issues or file system errors.
- **Programmer Errors:** Bugs in code, e.g., undefined variables.

### 🛠️ Combined Error Handling Examples

```js
// Synchronous error handling using try-catch
try {
  // Code that may throw an error
  throw new Error('Something went wrong');
} catch (err) {
  console.error('Caught error:', err.message);
}

// Asynchronous error handling with callback
const fs = require('fs');
fs.readFile('file.txt', (err, data) => {
  if (err) {
    console.error('Read error:', err);
    return;
  }
  console.log(data.toString());
});

// Error handling with Promises using .catch()
someAsyncFunction()
  .then(result => console.log(result))
  .catch(err => console.error('Promise error:', err));

// Async-await with try-catch
async function run() {
  try {
    const data = await someAsyncFunction();
    console.log(data);
  } catch (err) {
    console.error('Async error:', err);
  }
}
run();

// Handling uncaught exceptions and unhandled promise rejections
process.on('uncaughtException', (err) => {
  console.error('Uncaught Exception:', err);
  process.exit(1); // Exit or restart app safely
});

process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason);
});



## Interview Questions
### Beginner (1–30)

1. **What is Node.js?**
   Node.js is a runtime environment that allows executing JavaScript outside the browser, built on Chrome’s V8 engine for server-side development.

2. **What are the features of Node.js?**
   Features include event-driven, non-blocking I/O, single-threaded with event loop, cross-platform, fast execution, and npm package manager.

3. **How does Node.js handle asynchronous operations?**
   It uses an event-driven architecture with callbacks, promises, or async/await, enabling non-blocking I/O.

4. **What is the difference between Node.js and JavaScript in the browser?**
   Node.js runs JavaScript on the server with access to OS and file system, while browser JS runs client-side with DOM APIs.

5. **What is the event loop in Node.js?**
   The event loop processes asynchronous callbacks, managing tasks and ensuring non-blocking behavior.

6. **Explain the role of the V8 engine in Node.js.**
   V8 compiles JavaScript into native machine code for fast execution.

7. **What is npm?**
   npm (Node Package Manager) manages packages/modules for Node.js projects.

8. **How do you create a simple HTTP server in Node.js?**
   Use the `http` module:

   ```js
   const http = require('http');
   const server = http.createServer((req, res) => {
     res.end('Hello World');
   });
   server.listen(3000);

9. **What are CommonJS modules?**
   A module system using `require()` and `module.exports` to import/export code.

10. **What is the difference between `require` and `import`?**
    `require` is CommonJS (synchronous), `import` is ES Modules (supports static analysis and asynchronous loading).

11. **How do you handle errors in Node.js?**
    Use try/catch blocks, error-first callbacks, or `.catch()` on promises.

12. **What is callback hell and how do you avoid it?**
    Nested callbacks making code unreadable; avoid with promises or async/await.

13. **What are promises?**
    Objects representing eventual completion/failure of async operations.

14. **How does async/await work?**
    Syntax sugar over promises, making async code look synchronous.

15. **What is the difference between synchronous and asynchronous functions?**
    Sync blocks execution until complete; async runs in background allowing other tasks.

16. **Explain the concept of streams in Node.js.**
    Streams allow reading/writing data piece by piece rather than all at once.

17. **What is the purpose of the `Buffer` class?**
    Handles binary data directly.

18. **How can you read and write files in Node.js?**
    Using `fs` module methods like `fs.readFile()`, `fs.writeFile()`, or streams.

19. **What is the role of the `process` object?**
    Provides info about the current Node.js process and control over it.

20. **How can environment variables be accessed in Node.js?**
    Via `process.env.VARIABLE_NAME`.

21. **What is the difference between `process.nextTick()` and `setImmediate()`?**
    `nextTick` executes before next event loop phase; `setImmediate` runs on the next iteration.

22. **What are events and event emitters in Node.js?**
    Events signal occurrences; `EventEmitter` allows listening and emitting events.

23. **How do you handle uncaught exceptions?**
    Use `process.on('uncaughtException', handler)` or domain module (deprecated).

24. **What is the difference between `__dirname` and `process.cwd()`?**
    `__dirname` is the directory of the current module, `process.cwd()` is the current working directory.

25. **What is the `package.json` file?**
    Metadata about the project, including dependencies and scripts.

26. **How do you manage dependencies with npm?**
    Using `npm install package`, saved in `package.json`.

27. **What is middleware?**
    Functions executed during request handling, often used in Express.js.

28. **What is the difference between PUT and POST HTTP methods?**
    POST creates resources; PUT updates or creates at a specific URI.

29. **How do you create a simple REST API with Node.js?**
    Use `http` module or frameworks like Express to handle routes and methods.

30. **What is the difference between Node.js and Express.js?**
    Node.js is a runtime; Express.js is a web framework built on Node.js.

---

### Intermediate (31–70)

31. **Explain the Node.js module system.**
    Modules encapsulate code; CommonJS uses `require`/`exports`, ES Modules use `import`/`export`.

32. **What are ES Modules (ESM) and how do they differ from CommonJS?**
    ESM supports static imports, asynchronous loading, and is part of JS standard.

33. **What are worker threads?**
    Threads allowing CPU-intensive tasks in parallel without blocking the event loop.

34. **What is clustering in Node.js?**
    Running multiple Node.js processes to utilize multiple CPU cores.

35. **How does Node.js handle child processes?**
    Via `child_process` module using `fork()`, `spawn()`, or `exec()`.

36. **What is the `Event Loop` phases?**
    Timers, I/O callbacks, idle/prepare, poll, check, close callbacks.

37. **What are microtasks and macrotasks?**
    Microtasks (e.g. Promises) run immediately after the current task; macrotasks (e.g. timers) run later.

38. **Explain how garbage collection works in Node.js.**
    V8 engine uses generational GC to free unused memory automatically.

39. **What is `libuv`?**
    A C library that handles Node.js’s async I/O and event loop.

40. **How does Node.js achieve non-blocking I/O?**
    Using event-driven callbacks and libuv’s thread pool.

41. **How can you handle file uploads in Node.js?**
    Using middleware like `multer` or streaming data.

42. **How do you secure a Node.js application?**
    Input validation, use HTTPS, avoid eval, sanitize data, helmet middleware.

43. **What is the use of the `crypto` module?**
    Provides cryptographic functions like hashing and encryption.

44. **What are streams, and what types are there in Node.js?**
    Readable, Writable, Duplex, and Transform streams.

45. **How do you implement authentication in a Node.js app?**
    Using sessions, JWT, OAuth, Passport.js.

46. **What is CORS and how do you handle it in Node.js?**
    Cross-Origin Resource Sharing; handled via headers or middleware like `cors`.

47. **How do you debug a Node.js application?**
    Using `node inspect`, Chrome DevTools, or VSCode debugger.

48. **What is the difference between `process.nextTick()` and `setTimeout()`?**
    `nextTick` runs before I/O events; `setTimeout` schedules timers after I/O.

49. **How do you create a custom event emitter?**
    Extend `EventEmitter` class and use `.emit()` and `.on()`.

50. **What are Promises, and how do you chain them?**
    Objects representing async results; chain with `.then()`.

51. **What is the difference between a callback and a promise?**
    Callback is function-based async; promise is an object-based async pattern.

52. **How do you handle concurrency in Node.js?**
    Via async programming, worker threads, clustering, or child processes.

53. **What is the role of the thread pool in Node.js?**
    Handles expensive async tasks in separate threads.

54. **How do you handle streams errors?**
    Listen for `error` event on streams.

55. **What is a memory leak in Node.js and how do you detect it?**
    Memory not released; detect using profiling tools like Chrome DevTools.

56. **How do you optimize Node.js performance?**
    Use caching, avoid blocking code, optimize DB queries, scale with clustering.

57. **What is the use of `async_hooks` module?**
    Track async resources lifecycle for debugging/profiling.

58. **How do you manage sessions in Node.js?**
    Using `express-session` or token-based auth (JWT).

59. **How can you implement rate limiting?**
    Using middleware like `express-rate-limit`.

60. **What is the use of the `cluster` module?**
    To fork multiple worker processes for multi-core utilization.

61. **How do you deploy Node.js applications?**
    On platforms like Heroku, AWS, Docker containers, or VPS.

62. **What is the difference between blocking and non-blocking code?**
    Blocking stops execution until complete; non-blocking allows other tasks to run.

63. **Explain the purpose of the `next()` function in Express middleware.**
    Passes control to the next middleware.

64. **How do streams help in handling large files?**
    By processing data chunk by chunk without loading entire file in memory.

65. **What is `process.env`?**
    Object containing environment variables.

66. **How do you write unit tests in Node.js?**
    Using frameworks like Mocha, Jest, or built-in `assert`.

67. **What are template literals in JavaScript?**
    String literals allowing embedded expressions with backticks.

68. **How do you handle uncaught promise rejections?**
    Using `process.on('unhandledRejection', handler)`.

69. **What is the `http2` module?**
    Node.js module to support HTTP/2 protocol.

70. **How do you implement WebSockets in Node.js?**
    Using libraries like `ws` or `socket.io`.

---

### Advanced / Hardcore (71–100)

71. **Explain the internal working of the Node.js event loop.**
    It cycles through phases processing timers, I/O callbacks, idle, poll, check, and close callbacks to manage async tasks.

72. **How does Node.js handle thread safety?**
    Single-threaded JavaScript avoids data races; native modules must handle thread safety explicitly.

73. **What is the difference between `setImmediate()`, `setTimeout()`, and `process.nextTick()` in terms of event loop phases?**
    `nextTick` runs before event loop continues, `setImmediate` runs after I/O polling, `setTimeout` runs after timers phase.

74. **How can you implement a zero-downtime restart in Node.js?**
    Using process managers like PM2 with cluster mode or graceful shutdown and spawning new processes.

75. **What are native addons in Node.js?**
    Modules written in C/C++ compiled to work with Node.js for performance or system-level tasks.

76. **How does V8 optimize JavaScript execution?**
    Through JIT compilation, inline caching, and hidden classes.

77. **Explain the difference between asynchronous and deferred task queues in Node.js.**
    Async queues handle callbacks; deferred queues handle promises/microtasks.

78. **How do you build a scalable Node.js app?**
    By using clustering, load balancing, stateless architecture, caching, and microservices.

79. **What is the role of the `async_hooks` API?**
    To trace async operations for debugging and profiling.

80. **How do you debug memory leaks in Node.js?**
    Using heap snapshots and tools like Chrome DevTools or `clinic.js`.

81. **What is the role of libuv's thread pool?**
    To execute blocking or CPU-bound operations asynchronously.

82. **How do you implement backpressure in Node.js streams?**
    By managing the flow using `.pause()` and `.resume()` or the `highWaterMark` option.

83. **How do you handle CPU-intensive tasks in Node.js?**
    Offload to worker threads, child processes, or external services.

84. **What are the limitations of Node.js?**
    Single-threaded nature, not ideal for CPU-bound tasks, callback complexity.

85. **How do you secure a Node.js application from common vulnerabilities like SQL injection or XSS?**
    Validate input, use parameterized queries, sanitize output, use security middleware.

86. **Explain the role of `domain` module and why it is deprecated.**
    Was used for error handling in async code, deprecated due to complexity and alternatives.

87. **How does the garbage collector work in V8 and how can you tune it?**
    Uses generational GC with marking and sweeping; tune with flags like `--max-old-space-size`.

88. **What is the difference between fork() and spawn() in Node.js child processes?**
    `fork()` spawns new Node.js processes with IPC; `spawn()` launches any command without IPC.

89. **How do you implement microservices with Node.js?**
    Separate services communicating via REST, message queues, or gRPC.

90. **How do you implement caching in Node.js?**
    Using in-memory stores like Redis or built-in memory cache.

91. **How do you handle file system watchers efficiently?**
    Use `fs.watch` or third-party modules like `chokidar` with throttling/debouncing.

92. **What is the use of the `worker_threads` module vs `child_process`?**
    `worker_threads` share memory with main thread, lighter-weight for CPU tasks; `child_process` are separate processes.

93. **How do you handle streaming large JSON data?**
    Parse JSON streams using libraries like `JSONStream`.

94. **Explain the difference between buffer pooling and buffer allocation.**
    Buffer pooling reuses memory for efficiency; allocation creates new memory each time.

95. **What are the best practices for writing scalable and maintainable Node.js code?**
    Modularization, error handling, async best practices, logging, testing, and documentation.

96. **How do you implement graceful shutdown of a Node.js server?**
    Listen to signals (`SIGINT`), close connections, stop accepting new requests, and cleanup.

97. **How can you mitigate the risk of Denial of Service (DoS) attacks in Node.js?**
    Rate limiting, input validation, firewalls, and limiting request body size.

98. **What is the role of `async_hooks` in async context propagation?**
    It tracks async resources to maintain context across async calls.

99. **How do you handle cross-process communication in Node.js?**
    Using IPC channels, message queues, or shared databases.

100. **Explain how to profile a Node.js application for performance bottlenecks.**
     Using built-in profiler, `clinic.js`, Chrome DevTools, or `--inspect` flag.
