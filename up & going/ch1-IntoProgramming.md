# You Don't Know JS: Up & Going
# Chapter 1: Into Programming
[link](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20%26%20going/ch1.md)

## Code

### Statements

`a = b * 2;` a statement

### Expressions

`2` literal value expression  
`b` variable expression  
`b * 2` arithmetic expression  
`a = b * 2` assignment expression  

`b * 2` expression statement (not useful)  
`alert( a )` call expression statement (useful)

### Executing a Program

translation of code:  
every time the program is run -> interpreting  
ahead of time -> compiling  

JavaScript engines -> compile on the fly, immediately run

## Input

```js
age = prompt( "Please tell me your age:" );
console.log( age );
```

## Operators

`=` assignment  
`+ - * /` maths  
`+= -= *= /=` compound, combining assignment and math  
`++ --` increment/decrement  
`.` object property access, `obj.a` same as `obj["a"]`  

equality:  
* `==` loose-equals  
* `===` strict-equals  
* `!=` loose not-equals  
* `!==` strict not-equals  

comparison:  
* `< >` less/greater than  
* `<= >=` less/greater than or loose-equals  
`&& ||` logical  

## Values & Types

values are called literals  
`'string'` string literal  
`42` number literal  
`true` boolean literal  
others: `arrays, objects, functions`  

### Converting Between Types

`number` to `string` is coercion  
`string` to `number`:  
```js
var a = "42";
var b = Number( a );
```
`"99.99" == 99.99` is `true` in javascript because loose-equals converts types  

## Code Comments

* comment your code
* too many comments is a sign of bad code
* explain *why*, not *what*, maybe *how*

```js
// single-line comment

/* a multiline
    comment.
        */
```

multiline can be in the middle of a line:  
```js
var a = /* arbitrary value */ 42;
```

## Variables

when variables hold a specific type of value: *static typing*, or *type enforcement*  
-> prevents unintended conversions  

when variables can hold any type of value: *weak typing*, or *dynamic typing*  
-> program flexibility  
-> used by Javascript  

constants are variables not meant to be changed, when declared in one place it's easy to change them later.  
they're usually written in all caps with underscores:  
```js
var TAX_RATE = 0.08;
```
es6 allows const, value can't be changed by program:  
```js
const TAX_RATE = 0.08;
```

## Blocks

a block (valid but pointless on its own):  
```js
{
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

is usually attached to a control statement:  
```js
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

## Conditionals

`if` statement is the most common conditional  
`if` expects a `boolean` and will coerce to get it  
`0` and `""` are "falsy", they are false when coerced to a `boolean`  

## Loops

a loop repeats a set of actions until its condition fails  
includes a test condition followed by a block, each time it executes is an *iteration*  

`while` and `do..while` loops:  
```js
// tests conditional before iteration
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );
	numOfCustomers = numOfCustomers - 1;
}

// tests conditional after iteration, will always run at least once
do {
	console.log( "How may I help you?" );
	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```

introduce `break` statement to stop loop:  
```js
while (true) {
	if ((i > 9)) {
		break;
	}
	console.log( i );	// 0 1 2 3 4 5 6 7 8 9
	i = i + 1;
}
```

`for` loop can accomplish the same thing:  
```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );	// 0 1 2 3 4 5 6 7 8 9
}
```
`for` loop has three clauses:  
* initialization clause (`var i=0`), runs before first iteration  
* conditional test clause (`i <= 9`), tested before every iteration  
* update clause (`i = i + 1`), executed after every iteration  

## Functions

a function is a usually named block of code that can be called and executed  
optionally takes arguments and can return a value  

functions can be used for code that's to be called multiple times  
can also just be used to oragnize bits of code into named collections  


