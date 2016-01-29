# Javascript

I use many small blocks of Javascript in the
[wmt-client](https://github.com/csdms/wmt-client);
e.g., see
[ParameterJSO.java](https://github.com/csdms/wmt-client/blob/master/src/edu/colorado/csdms/wmt/client/data/ParameterJSO.java).


## Documentation

* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)


## Node.js

I've installed [Node.js](https://nodejs.org/en/)
on ***solaria*** through Homebrew
(package name = `nodejs`).
I can use it to call a script from the command line.

For example, if I have a script **foo.js**:

```javascript
console.log('hi');
```

I can call it with `node`:

```bash
$ node foo.js
hi
```


## Get the keys of an object

Or, equivalently, the property names of an object.
Use `Object.keys()`:

```javascript
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']
```

Also use this to count the number of properties:
```javascript
console.log(Object.keys(obj).length); // console: 3
```

**Reference:**

* [Object.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) [developer.mozilla.org]
