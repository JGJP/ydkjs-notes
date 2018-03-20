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

## Conditionals

`switch` without `break` after each `case` results in "fall through", sometimes useful/desired:

```js
switch (a) {
	case 2:
	case 10:
		// some cool stuff when a is 2 OR 10
		break;
	case 42:
		// other stuff for 42
		break;
	default:
		// fallback
}
```

Another conditional is the "conditional operator" or "ternary operator". More concise form of `if..else` statement:

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";
```

During assignment is the most common usage.

## Strict Mode

ES5 added a "strict mode", tightens rules for certain behaviors, for safety, as guidelines, and better optimization by the engine.  
Strict mode is always preferable.  

You can opt in to strict mode for an individual function, or an entire file, depending on where you put the strict mode pragma.

Function:

```js
function foo() {
	"use strict";

	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is not strict mode
```

Entire file:

```js
"use strict";

function foo() {
	// this code is strict mode

	function bar() {
		// this code is strict mode
	}
}

// this code is strict mode
```

A key benefit is disallowing the implicit auto-global variable declaration from omitting `var`:

```js
function foo() {
	"use strict";	// turn on strict mode
	a = 1;			// `var` missing, ReferenceError
}

foo();
```

## Functions Are Values

```js
function foo() {
	// ..
}

// same as

var foo = function(){
	// anonymous function assigned to foo variable
}

```

### Immediately Invoked Function Expressions (IIFEs)

*immediately invoked function expression* (IIFE):

```js
(function IIFE(){
	console.log( "Hello!" );
})();
// "Hello!"
```

The outer `( .. )` that surrounds the function expression is a nuance of JS grammar to prevent it from being treated as a normal function declaration.

The final `()` on the end of the expression is what executes the function expression before it.

Using an IIFE in this fashion is often used to declare variables that won't affect the surrounding code:

```js
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42
```

IIFEs can also have return values:

```js
var x = (function IIFE(){
	return 42;
})();

x;	// 42
```

The `42` value gets `return`ed from the `IIFE`-named function being executed, and is then assigned to `x`.

### Closure

You can think of closure as a way to "remember" and continue to access a function's scope (its variables) even once the function has finished running:

```js
function makeAdder(x) {
	// parameter `x` is an inner variable

	// inner function `add()` uses `x`, so
	// it has a "closure" over it
	function add(y) {
		return y + x;
	};

	return add;
}
```

The reference to the inner `add(..)` function that gets returned with each call to the outer `makeAdder(..)` is able to remember whatever `x` value was passed in to `makeAdder(..)`:

```js
var plusOne = makeAdder( 1 );

var plusTen = makeAdder( 10 );

plusOne( 3 );		// 4  <-- 1 + 3
plusOne( 41 );		// 42 <-- 1 + 41

plusTen( 13 );		// 23 <-- 10 + 13
```

#### Modules

* the most common usage of closure  is the module pattern
* variables, functions can be hidden from the outside world
* can expose a public API that *is* accessible from the outside

```js
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

Only the return value (`publicAPI`) is available to the outside world.  

`User()` is just a function, not a class to be instantiated, so it's just called normally.  
Using `new` would be inappropriate and would actually waste resources.

Executing `User()` creates an *instance* of the `User` module -- a whole new scope is created and assigned to `fred`  

The inner `doLogin()` function has a closure over `username` and `password`, meaning it will retain its access to them even after the `User()` function finishes running.

`publicAPI` is an object with one property/method on it, `login`, which is a reference to the inner `doLogin()` function. When we return `publicAPI` from `User()`, it becomes the instance we call `fred`.

At this point, the outer `User()` function has finished executing. Normally, you'd think the inner variables like `username` and `password` have gone away. But here they have not, because there's a closure in the `login()` function keeping them alive.

That's why we can call `fred.login(..)` -- the same as calling the inner `doLogin(..)` -- and it can still access `username` and `password` inner variables.

## `this` Identifier

If a function has a `this` reference inside it, that `this` reference usually points to an `object`. But which `object` it points to depends on how the function was called.

```js
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );		// "obj2"
new foo();			// undefined
```

1. `foo()` ends up setting `this` to the global object in non-strict mode -- in strict mode, `this` would be `undefined` and you'd get an error in accessing the `bar` property -- so `"global"` is the value found for `this.bar`.
2. `obj1.foo()` sets `this` to the `obj1` object.
3. `foo.call(obj2)` sets `this` to the `obj2` object.
4. `new foo()` sets `this` to a brand new empty object.

These are the only four ways that `this` can be set.  

## Prototypes

When you reference a property on an object, if that property doesn't exist, JavaScript will automatically use that object's internal prototype reference to find another object to look for the property on.

The internal prototype reference linkage from one object to its fallback happens at the time the object is created. The simplest way to illustrate it is with a built-in utility called `Object.create(..)`.

```js
var foo = {
	a: 42
};

// create `bar` and link it to `foo`
var bar = Object.create( foo );

bar.b = "hello world";

bar.b;		// "hello world"
bar.a;		// 42 <-- delegated to `foo`
```

The `a` property doesn't actually exist on the `bar` object, but because `bar` is prototype-linked to `foo`, JavaScript automatically falls back to looking for `a` on the `foo` object, where it's found.

This linkage may seem like a strange feature of the language. The most common way this feature is used -- and I would argue, abused -- is to try to emulate/fake a "class" mechanism with "inheritance."

But a more natural way of applying prototypes is a pattern called "behavior delegation," where you intentionally design your linked objects to be able to *delegate* from one to the other for parts of the needed behavior.

## Old & New

There are two main techniques you can use to "bring" the newer JavaScript stuff to the older browsers: polyfilling and transpiling.

### Polyfilling

The word "polyfill" was invented by [Remy Sharp](https://remysharp.com/2010/10/08/what-is-a-polyfill), it means taking the definition of a newer feature and producing a piece of code that's equivalent to the behavior, but is able to run in older JS environments.

For example, ES6 defines a utility called `Number.isNaN(..)` to provide an accurate non-buggy check for `NaN` values, deprecating the original `isNaN(..)` utility. But it's easy to polyfill that utility so that you can start using it in your code regardless of whether the end user is in an ES6 browser or not.

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

The `if` statement guards against applying the polyfill definition in ES6 browsers where it will already exist. If it's not already present, we define `Number.isNaN(..)`.

**Note:** The check we do here takes advantage of a quirk with `NaN` values, which is that they're the only value in the whole language that is not equal to itself. So the `NaN` value is the only one that would make `x !== x` be `true`.

Not all new features are fully polyfillable. Sometimes most of the behavior can be polyfilled, but there are still small deviations. You should be really, really careful in implementing a polyfill yourself, to make sure you are adhering to the specification as strictly as possible.

Trusted polyfills: ES5-Shim (https://github.com/es-shims/es5-shim) and ES6-Shim (https://github.com/es-shims/es6-shim).
