# Node.js Features Explained with Examples

This document provides an in-depth look at key Node.js features, with explanations and code examples for each.

## 1. Asynchronous and Event-Driven

**Explanation:** Node.js uses an event-driven, non-blocking I/O model, making it lightweight and efficient. This allows Node.js to handle multiple connections concurrently.

**Example:**

```javascript
const fs = require('fs');

fs.readFile('example.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log('File contents:', data);
});

console.log('This will be printed before file contents');
```

## 2. Single-Threaded Event Loop

**Explanation:** Node.js operates on a single thread, using an event loop for asynchronous operations. This allows it to handle many concurrent operations without the overhead of multiple threads.

**Example:**

```javascript
const crypto = require('crypto');

process.nextTick(() => {
  console.log('NextTick callback');
});

setImmediate(() => {
  console.log('Immediate callback');
});

setTimeout(() => {
  console.log('Timeout callback');
}, 0);

crypto.randomBytes(256, (err, buf) => {
  if (err) throw err;
  console.log('Crypto callback');
});

console.log('Main program finished');
```

## 3. NPM (Node Package Manager)

**Explanation:** NPM is the default package manager for Node.js, allowing easy sharing and reuse of code through packages.

**Example:**

```javascript
// package.json
{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}

// Install dependencies
// $ npm install

// Usage in code
const express = require('express');
const app = express();
```

## 4. CommonJS Module System

**Explanation:** Node.js uses the CommonJS module system for organizing and reusing code.

**Example:**

```javascript
// math.js
module.exports = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b
};

// app.js
const math = require('./math');
console.log(math.add(5, 3));  // Output: 8
```

## 5. Buffer Class

**Explanation:** The Buffer class in Node.js is used to handle binary data directly.

**Example:**

```javascript
const buf1 = Buffer.alloc(10);  // Create a buffer of 10 bytes
const buf2 = Buffer.from('Hello, World!');  // Create a buffer from a string

console.log(buf1);  // <Buffer 00 00 00 00 00 00 00 00 00 00>
console.log(buf2.toString());  // Hello, World!
```

## 6. Stream API

**Explanation:** Streams are objects that let you read data from a source or write data to a destination in a continuous fashion.

**Example:**

```javascript
const fs = require('fs');

const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt');

readStream.pipe(writeStream);

readStream.on('end', () => {
  console.log('Read and write completed');
});
```

## 7. Cluster Module

**Explanation:** The cluster module allows easy creation of child processes that all share server ports, enabling load balancing across multiple CPU cores.

**Example:**

```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  console.log(`Master ${process.pid} is running`);

  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died`);
  });
} else {
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('Hello World\n');
  }).listen(8000);

  console.log(`Worker ${process.pid} started`);
}
```

## 8. Child Processes

**Explanation:** Node.js provides the ability to spawn child processes, allowing you to execute external commands.

**Example:**

```javascript
const { spawn } = require('child_process');

const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`Child process exited with code ${code}`);
});
```

## 9. Global Objects

**Explanation:** Node.js provides several global objects that are available in all modules.

**Example:**

```javascript
console.log(__filename);  // Full path of the current file
console.log(__dirname);   // Directory name of the current file
console.log(process.env); // Environment variables

process.on('exit', (code) => {
  console.log(`About to exit with code: ${code}`);
});
```

## 10. Error Handling

**Explanation:** Node.js provides mechanisms for handling and propagating errors.

**Example:**

```javascript
process.on('uncaughtException', (err) => {
  console.error('Uncaught Exception:', err);
  process.exit(1);
});

try {
  throw new Error('An error occurred');
} catch (err) {
  console.error('Caught error:', err);
}

// Asynchronous error handling
fs.readFile('nonexistent.txt', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  // Process data
});
```

## 11. HTTP/HTTPS Module

**Explanation:** Node.js has built-in modules for creating HTTP and HTTPS servers and making HTTP/HTTPS requests.

**Example:**

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(3000, '127.0.0.1', () => {
  console.log('Server running at http://127.0.0.1:3000/');
});
```

## 12. File System Operations

**Explanation:** Node.js provides a File System (fs) module for interacting with the file system.

**Example:**

```javascript
const fs = require('fs');

// Synchronous read
const data = fs.readFileSync('input.txt', 'utf8');
console.log(data);

// Asynchronous write
fs.writeFile('output.txt', 'Hello, Node.js!', (err) => {
  if (err) throw err;
  console.log('File has been saved');
});
```

## 13. Path Module

**Explanation:** The Path module provides utilities for working with file and directory paths.

**Example:**

```javascript
const path = require('path');

console.log(path.join('/home', 'user', 'docs', 'file.txt'));
// Output: /home/user/docs/file.txt

console.log(path.resolve('folder', 'subfolder', '..', 'file.txt'));
// Output: /current/working/directory/folder/file.txt

console.log(path.extname('index.html'));
// Output: .html
```

## 14. URL Module

**Explanation:** The URL module provides utilities for URL resolution and parsing.

**Example:**

```javascript
const url = require('url');

const myURL = new URL('https://example.org/foo?bar=baz');

console.log(myURL.hostname);  // example.org
console.log(myURL.pathname);  // /foo
console.log(myURL.searchParams.get('bar'));  // baz
```

## 15. Event Emitter

**Explanation:** Many Node.js core APIs are built around an idiomatic asynchronous event-driven architecture where certain kinds of objects (called "emitters") emit named events.

**Example:**

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

myEmitter.on('event', () => {
  console.log('An event occurred!');
});

myEmitter.emit('event');
```

## 16. Crypto Module

**Explanation:** The Crypto module provides cryptographic functionality, including a set of wrappers for OpenSSL's hash, HMAC, cipher, decipher, sign, and verify functions.

**Example:**

```javascript
const crypto = require('crypto');

const secret = 'abcdefg';
const hash = crypto.createHmac('sha256', secret)
                   .update('I love cupcakes')
                   .digest('hex');
console.log(hash);
```

## 17. Process Object

**Explanation:** The process object provides information about, and control over, the current Node.js process.

**Example:**

```javascript
console.log(`Node.js version: ${process.version}`);
console.log(`Process platform: ${process.platform}`);
console.log(`Process ID: ${process.pid}`);

process.on('exit', (code) => {
  console.log(`About to exit with code: ${code}`);
});

process.exit(0);
```

These examples and explanations cover the key features of Node.js, demonstrating its power and flexibility in building server-side applications. Node.js's event-driven, non-blocking I/O model, along with its rich set of core modules and vast ecosystem of third-party packages, make it a powerful platform for creating efficient and scalable network applications.
