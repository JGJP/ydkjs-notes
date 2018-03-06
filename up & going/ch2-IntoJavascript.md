# You Don't Know JS: Up & Going
# Chapter 2: Into JavaScript
[link](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch2.md)

## Values & Types

Use `typeof` operator to find type of a variable's value:

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

Bracket notation is useful for property names with special characters in it, like `obj["hello world!"]` -- referred to as *keys* when accessed via bracket notation. The `[ ]` notation requires either a variable (explained next) or a `string` *literal* (which needs to be wrapped in `" .. "` or `' .. '`).

Bracket notation is also useful for accessing a property/key whose name is stored in another variable:

```js
var obj = {
	a: "hello world",
	b: 42
};

var b = "a";

obj[b];			// "hello world"
obj["b"];		// 42
```

Value subtypes: *array* and *function*, are specialized versions of the `object` type.

#### Arrays

An array is an `object` that holds values in numerically indexed positions.

```js
var arr = [
	"hello world",
	42,
	true
];

arr[0];			// "hello world"
arr[1];			// 42
arr[2];			// true
arr.length;		// 3

typeof arr;		// "object"
```

Arrays are objects, so can also have properties, like the automatically updated `length` property.

#### Functions

Functions are another `object` subtype.

```js
function foo() {
	return 42;
}

foo.bar = "hello world";

typeof foo;			// "function"
typeof foo();		// "number"
typeof foo.bar;		// "string"
```

### Built-In Type Methods

The built-in types and subtypes we've just discussed have behaviors exposed as properties and methods.

```js
var a = "hello world";
var b = 3.14159;

a.length;				// 11
a.toUpperCase();		// "HELLO WORLD"
b.toFixed(4);			// "3.1416"
```

There is a `String` object wrapper form, typically called a "native," that pairs with the primitive `string` type.  
This object wrapper defines the `toUpperCase()` method on its prototype.  

When you use a primitive value like `"hello world"` as an `object` by referencing a property or method, JS automatically "boxes" the value to its object wrapper counterpart.  

* `string` value wrapped by a `String` object
* `number` value wrapped by a `Number` object
* `boolean` value wrapped by a `Boolean` object

### Comparing Values

the result of any comparison is always a `boolean` value.

#### Coercion

* *explicit* coercion: obvious that a conversion will occur  

```js
var a = "42";
var b = Number( a );
a;				// "42"
b;				// 42
```

* *implicit* coercion: non-obvious side effect causes conversion

```js
var a = "42";
var b = a * 1;	// "42" implicitly coerced to 42 here
a;				// "42"
b;				// 42
```
