# Quizz 1

Using you're knowledge of the event loop, create a program which prints out the below. If the log line mentions a `setInterval` it must be printed inside a `setInterval` etc..

start
end
setInterval 1
promise 1
promise 2

```js
console.log('start');
const interval = setInterval(() => {
  console.log('setInterval 1');
  Promise.resolve()
    .then(_ => console.log('promise 1'))
    .then(_ => {
      console.log('promise 2');
      setImmediate(_ => {
        console.log('setImmediate 1');
        Promise.resolve()
          .then(_ => console.log('promise 3'))
          .then(_ => console.log('promise 4'));
      });
      process.nextTick(_ => console.log('processNextTick 1'));
      clearInterval(interval);
    });
});
console.log('end');
```

# Quizz 2

Extend the previous example to print out the following log lines, use `process.nextTick` and `setImmediate`

start
end
setInterval 1
promise 1
promise 2
processNextTick 1
setImmediate 1
promise 3
promise 4
