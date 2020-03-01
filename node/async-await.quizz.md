# Question 1

Convert the promise version of the multi-file loader over to using async/await

```js
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const files = ["./files/demofile.txt", "./files/demofile.other.txt"];

function readFiles(files) {
  let promises = files.map(name => readFile(name, { encoding: "utf8" }));
  return Promise.all(promises);
}
(async _ => {
  const values = await readFiles(files);
  console.log(values);
})();
(async () => {
  for (const file of files) {
    let value = await readFile(file, "utf8");
    console.log(value);
  }
})();
```

# Question 2

Again convert the promise version of the multi-file loader over to using async/await but using a custom async iterator with the following syntax

node --harmony-async-iteration <file.js>

```js
const fs = require("fs");
const util = require("util");
const readFile = util.promisify(fs.readFile);

const fileIterator = files => ({
  [Symbol.asyncIterator]: _ => ({
    x: 0,
    next() {
      if (this.x > files.length - 1) return { done: true };

      let file = files[this.x++];

      return readFile(file, "utf8").then(data => ({
        done: false,
        value: data
      }));
    }
  })
});
const files = ["./files/demofile.txt", "./files/demofile.other.txt"];
(async () => {
  for await (let fileContents of fileIterator(files)) console.log(fileContents);
})();
```
