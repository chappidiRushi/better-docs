
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
