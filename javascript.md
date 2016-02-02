# Javascript

I use many small blocks of Javascript in the
[wmt-client](https://github.com/csdms/wmt-client);
e.g., see
[ParameterJSO.java](https://github.com/csdms/wmt-client/blob/master/src/edu/colorado/csdms/wmt/client/data/ParameterJSO.java).

Use these variables in the examples below:
```javascript
var model = {
	'date': "2014-05-16T17:24:26",
	'owner': "huttone@colorado.edu",
	'id': 84,
	'name': "CEM + Waves + Avulsion + River"
};
var components = [
  "channels_diffusive_wave",
  "channels_dynamic_wave",
  "channels_kinematic_wave",
];
```

## Documentation

* [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)


## Node.js

I've installed [Node.js](https://nodejs.org/en/)
on ***solaria*** through Homebrew
(package name = `nodejs`).
I can use it to call a script from the command line.

For example, if I have a script **foo.js**
that contains the code:
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
console.log(Object.keys(model));
// [ 'date', 'owner', 'id', 'name' ]
```

**Reference:**

* [Object.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) [developer.mozilla.org]


## Count the properties of an object

Use the `length` property.

For an object:
```javascript
console.log(Object.keys(model).length);
// 4
```

For an array:
```javascript
console.log(components.length);
// 3
```


## Check for an undefined property

Use the `typeof` operator, which returns a string; e.g.:

```javascript
var n = 0;
if (typeof this.uses != 'undefined') {
	n = Object.keys(this.uses).length;
}
return n;
```

## Check for membership in an object

Use the `in` operator:
```javascript
console.log('owner' in model);
// true
```
