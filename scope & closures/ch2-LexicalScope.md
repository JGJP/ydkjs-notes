# You Don't Know JS: Scope & Closures
# Chapter 2: Lexical Scope
[link](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch2.md)

## Lex-time

* The first phase of a compiler is called lexing (aka, tokenizing).
* Lexing assigns semantic meaning to the tokens parsed from a raw string of source code.
* Lexical scope is scope that is defined at lexing time.

### Look-ups

* Scope look-up starts at the innermost scope being executed and works outward.
* Scope look-up stops once it finds the first match for a variable.
* **Shadowing** is when a variable is defined in multiple nested scopes.
* An inner identifier **shadows** an outer identifier.

Global variables are properties of the global object (`window` in browsers)

```js
window.a
```

* Even if `a` is shadowed, it can be accessed via `window.a`
* Non-global shadowed variables cannot be accessed from inner scopes

Lexical scope is only defined by where a function is declared, not where it's invoked from.

## Cheating Lexical

Modifying lexical scope at run-time leads to poorer performance.  

Two ways to modify (cheat) lexical scope at run-time:  

### `eval`

`eval()` takes a string and treats it like code that was originally written.  

*Engine* won't know or care whether the code was originally there or is a result of `eval`.

```js
function foo(str, a) {
	eval( str ); // cheating! new variable b shadows global b
	console.log( a, b );
}

var b = 2;

foo( "var b = 3;", 1 ); // 1 3
```

`"use strict"` prevents this from happening:  

```js
function foo(str) {
   "use strict";
   eval( str ); // a is declared in eval's own separate scope
   console.log( a ); // ReferenceError: a is not defined
}

foo( "var a = 2" );
```

A similar effect can be achieved through `setTimeout(..)` and `setInterval(..)`.  
They evaluate strings passed in as a first argument, this is legacy behavior.

The `new Function(..)` function constructor will evaluate a string passed as its ***last*** argument.

### `with`

Usage of `with`:

```js
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// more "tedious" to repeat "obj"
obj.a = 2;
obj.b = 3;
obj.c = 4;

// "easier" short-hand
with (obj) {
	a = 3;
	b = 4;
	c = 5;
}
```

How it can go wrong:

```js
function foo(obj) {
	with (obj) {
		a = 2;
	}
}

var o1 = {
	a: 3
};

var o2 = {
	b: 3
};

foo( o1 );
console.log( o1.a ); // 2

foo( o2 );
console.log( o2.a ); // undefined
console.log( a ); // 2 -- Oops, leaked global!
```

`with` makes a new scope out of `obj`, but if a LHS lookup fails within that scope, it creates a new variable in global scope.

Strict mode disallows usage of `with`.

### Performance

*Engine* will not perform certain runtime optimizations when it encounters `eval()` or `with`, so it's also better for performance to never use them.