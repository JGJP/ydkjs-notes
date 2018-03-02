# You Don't Know JS: Up & Going
# Chapter 2: Into JavaScript
[link](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md)

## Values & Types

use `typeof` operator to find type of a variable's value:

```js
var a;
typeof a;				// "undefined"

a = "hello world";
typeof a;				// "string"

a = 42;
typeof a;				// "number"

a = true;
typeof a;				// "boolean"

a = null;
typeof a;				// "object" -- weird, bug, should be null

a = undefined;
typeof a;				// "undefined", same as if a was never declared

a = { b: "c" };
typeof a;				// "object"
```

### Objects

`object` is a compound value where you can set properties (named locations) that each hold their own values of any type

```js
var obj = {
	a: "hello world",
	b: 42,
	c: true
};

obj.a;		// "hello world"
obj.b;		// 42
obj.c;		// true

obj["a"];	// "hello world"
obj["b"];	// 42
obj["c"];	// true
```

bracket notation useful for property names with special characters in it, like `obj["hello world!"]` -- referred to as *keys* when accessed via bracket notation. The `[ ]` notation requires either a variable (explained next) or a `string` *literal* (which needs to be wrapped in `" .. "` or `' .. '`).

bracket notation also useful for accessing a property/key whose name is stored in another variable:

```js
var obj = {
	a: "hello world",
	b: 42
};

var b = "a";

obj[b];			// "hello world"
obj["b"];		// 42
```

value subtypes: *array* and *function*, are specialized versions of the `object` type.