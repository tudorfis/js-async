# Question 1 - (10min)

Create a promise version of the async readFile function

```js
const fs = require("fs");
const util = require("util");
const readFilePromise = util.promisify(fs.readFile);

function readFile(filename, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, encoding, (err, data) => {
      if (err) reject(err);
      resolve(data);
    });
  });
}
readFile("./files/demofile1.txt", "utf-8").then(
  data => console.log("Data: ", data),
  err => console.log("Error: ", err)
);
```

# Question 2

Load a file from disk using readFile and then compress it using the async zlib node library, use a promise chain to process this work.

```js
const fs = require("fs");
const zlib = require("zlib");

function zlibPromise(data) {
  return new Promise((resolve, reject) => {
    zlib.gzip(data, (error, result) => {
      if (error) reject(error);
      resolve(result);
    });
  });
}

function readFile(filename, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, encoding, (err, data) => {
      if (err) reject(err);
      resolve(data);
    });
  });
}

readFile("./files/demofile.txt", "utf-8")
  .then(data => zlibPromise(data))
  .then(res => console.log("Complete: ", res))
  .catch(err => console.log("error: ", err));
// --> Load it then zip it and then print it to screen
```

# Question 3

Convert the previous code so that it now chains the promise as well.

# Question 4

Convert the previous code so that it now handles errors using the catch handler

# Question 5

Create some code that tries to read from disk a file and times out if it takes longer than 1 seconds, use `Promise.race`

```js
function readFileFake(sleep) {
  return new Promise(resolve =>
    setTimeout(_ => {
      resolve("readFileFake");
    }, sleep)
  );
}

function timeout(sleep) {
  return new Promise((_, reject) => setTimeout(reject, sleep, "timeout msg"));
}

Promise.race([readFileFake(1000), timeout(5000)])
  .then(res => console.log(res))
  .catch(err => console.log(err));
```

# Question 6

Create a process flow which publishes a file from a server, then realises the user needs to login, then makes a login request, the whole chain should error out if it takes longer than 1 seconds. Use `catch` to handle errors and timeouts.

```js
function authenticate() {
  console.log("Authenticating");
  return new Promise(resolve => setTimeout(resolve, 2000, { status: 200 }));
}

function publish() {
  console.log("Publishing");
  return new Promise(resolve => setTimeout(resolve, 2000, { status: 403 }));
}

function timeout(sleep) {
  return new Promise((_, reject) => setTimeout(reject, sleep, "timeout"));
}

function safePublish() {
  return publish().then(res => {
    if (res.status === 403) return authenticate();

    return res;
  });
}

Promise.race([safePublish(), timeout(5000)])
  .then(_ => console.log("published"))
  .catch(err => {
    if (err === "timeout") console.log("Request timed out");
    else console.error("Error: ", err);
  });
```
