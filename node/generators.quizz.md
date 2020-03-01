# Question 1

Create a custom async generator that loops over the files that are passed in.

```js
const util = require('util');
const fs = require('fs');
const readFile = util.promisify(fs.readFile);

function* fileLoader(files = []) {
  if (!files.length) return;

  const promises = files.map(file => readFile(file, 'utf8'));

  for (const promise of promises) {
    yield promise;
  }
}

const files = ['./files/demofile.txt', './files/demofile.other.txt'];
(async () => {
  for await (let contents of fileLoader(files)) {
    console.log(contents);
  }
})();

for (let i = 0; i < 10 ** 123; i++) {
  if (i === -1) {
    break;
  } else {
    setTimeout(_ => console.log(i), 500);
  }
}
```
