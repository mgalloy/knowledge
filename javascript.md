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

**Reference:**

* [Object.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) [developer.mozilla.org]


## Count the properties of an object

Use the `length` property.

For an object:
```javascript
var model = {
	'date': "2014-05-16T17:24:26",
	'owner': "huttone@colorado.edu",
	'id': 84,
	'name': "CEM + Waves + Avulsion + River"
};
console.log(Object.keys(model).length); // console: 4
```

For an array:
```javascript
var components = [
  "channels_diffusive_wave", 
  "channels_dynamic_wave", 
  "channels_kinematic_wave", 
];
console.log(components.length); // console: 3
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
