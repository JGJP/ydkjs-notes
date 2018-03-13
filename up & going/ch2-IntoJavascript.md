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

#### Truthy & Falsy

"falsy" values:

* `""` (empty string)
* `0`, `-0`, `NaN` (invalid `number`)
* `null`, `undefined`
* `false`

Anything not on that list is "truthy":  

* `"hello"`
* `42`
* `true`
* `[ ]`, `[ 1, "2", 3 ]` (arrays)
* `{ }`, `{ a: 42 }` (objects)
* `function foo() { .. }` (functions)

#### Equality

* Four equality operators: `==`, `===`, `!=`, `!==`. 
* `!` forms are symmetric "not equal" versions of their counterparts
* *non-equality* should not be confused with *inequality*.

* `==` checks for value equality with coercion allowed
* `===` checks for value equality with coercion not allowed (strict equality)

`array`s are by default coerced to `string`s, but when comparing two objects, the references are compared, not contents.

```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

#### Inequality

* Relational comparison　operators, usually used for `number`s: `<`, `>`, `<=`, `>=`.
* Can also be used for `string` values, using typical alphabetic rules (`"bar" < "foo"`).
* Similar rules as `==` comparison apply to the inequality operators, but no `===` "strict equality" equivalent.

```js
var a = 41;
var b = "42";
var c = "43";

a < b;		// true, b coerced to number
b < c;		// true, no coercion, compared lexicographically
```

No "strict inequality" forms, `false` always returned when string coercion gives `NaN`:

```js
var a = 42;
var b = "foo";

a < b;		// false
a > b;		// false
a == b;		// false
```

## Variables

* Variable names (including function names) must be valid *identifiers*.
* An identifier must start with `a`-`z`, `A`-`Z`, `$`, or `_`. It can then contain any of those characters plus the numerals `0`-`9`.
* Certain words ("reserved words") cannot be used as variables, but are OK as property names: `for`, `in`, `if`, etc. as well as `null`, `true`, and `false`.

#### Hoisting

*hoisting* means a `var` is accessible anywhere within its scope, even before its declaration.  
This is common with functions but can be confusing for variables, best to avoid.  

```js
var a = 2;

foo();					// works because `foo()`
						// declaration is "hoisted"

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;				// declaration is "hoisted"
						// to the top of `foo()`
}

console.log( a );	// 2
```

#### Nested Scopes

```js
function foo() {
	var a = 1;

	function bar() {
		var b = 2;

		function baz() {
			var c = 3;

			console.log( a, b, c );	// 1 2 3
		}

		baz();
		console.log( a, b );		// 1 2
	}

	bar();
	console.log( a );				// 1
}

foo();
```

* Accessing a variable in a scope where it's not available: `ReferenceError`  
* Trying to set a variable that hasn't been declared will create a variable in the top-level global scope (bad!) or give an error, depending on "strict mode"  

```js
function foo() {
	a = 1;	// `a` not formally declared
}

foo();
a;			// 1 -- oops, auto global variable :(
```

* Always formally declare your variables.  
* ES6 *lets* you declare variables to belong to individual blocks (pairs of `{ .. }`), using `let`. Scoping rules are almost the same.  

```js
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

* Because of `let` instead of `var`, `b` belongs only to the `if` statement, not to the whole `foo()` function.  
* `c` also belongs only to the `while` loop.  
