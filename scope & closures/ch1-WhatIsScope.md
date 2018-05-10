# You Don't Know JS: Scope & Closures
# Chapter 1: What is Scope?
[link](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch1.md)

Where and how do *Scope* rules get set?

## Compiler Theory

* Javascript categorized as "dynamic / interpreted" language
* is actually compiled, but compilation results are not portable among systems
* compiled on the fly

"compilation":

1. **Tokenizing/Lexing:** breaking up a string into meaningful chunks (tokens). 

	`var a = 2;` becomes `var`, `a`, `=`, `2`, and `;`

2. **Parsing:** turning stream (array) of tokens into a tree of nested elements, representing the grammatical structure of the program. 

	This tree is called an "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree).

    Tree for `var a = 2;` ->  
    * top-level node: `VariableDeclaration`  
    * children: `Identifier` (value is `a`), `AssignmentExpression`  
    * child of `AssignmentExpression`: `NumericLiteral` (value is `2`).

3. **Code-Generation:** turning an AST into executable code (machine instructions). Varies depending on language, platform, etc.

## Understanding Scope

### The Cast

1. *Engine*: responsible for compilation and execution of a JavaScript program.

2. *Compiler*: handles parsing and code-generation.

3. *Scope*: collects and maintains look-up list of all declared identifiers (variables), enforces a strict set of rules as to how these are accessible to currently executing code.

### Back & Forth

*Engine*: `var a = 2;` -> two distinct statements, one for *Compiler* and one for *Engine*

1. `var a` -> *Compiler* asks *Scope* if `a` already exists in that particular scope collection.  
If it exists, *Compiler* ignores this declaration.  
If not, *Compiler* asks *Scope* to declare a new variable called `a` for that scope collection.

2. *Compiler* produces code for *Engine* to execute.  
`a = 2` assignment: *Engine* asks *Scope* if there is a variable called `a` accessible in the current scope collection.  
If so, *Engine* uses that variable. If not, *Engine* looks *elsewhere*.

If *Engine* eventually finds a variable, it assigns the value `2` to it.  
If not, *Engine* will throw an error.

To summarize: First, *Compiler* declares a variable (if not previously declared in the current scope), and second, when executing, *Engine* looks up the variable in *Scope* and assigns to it, if found.

### Compiler Speak

- *Engine* executes code that *Compiler* produced
- *Engine* consults *Scope* to look-up `a`

Types of lookup:

1. "RHS" (Right-hand Side) look-up
	- when a variable appears on the right-hand side of an assignment operation.
	- simply finds value, for use

2. "LHS" (Left-hand Side) look-up
	- when a variable appears on the left-hand side of an assignment operation
	- finds variable container, to assign

```js
console.log( a ); // RHS

a = 2; // LHS

foo( 2 ); // RHS of foo

// when foo is called
function foo(a) { // LHS (assigning 2 to a)
	console.log( a ); // RHS
}

function bar(){ /* ... */ } // no lookup, declaration and value definition are handled during compilation

var b = 2 // this is still LHS, because of assignment
```

### Quiz

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

Find:

1. LHS look-ups (there are 3).

2. RHS look-ups (there are 4).

solution:
```js
function foo(a) { // LHS
	var b = a; // LHS, RHS
	return a + b; // RHS, RHS
}

var c = foo( 2 ); // LHS, RHS
```

## Nested Scope

There are usually multiple scopes nested inside each other.  
When a variable is not found in the immediate scope, *Engine* checks the next outer scope, continuing until the variable is found.

## Errors

LHS and RHS behave differently when a variable is not found.

When a RHS lookup fails, *Engine* throws a `ReferenceError`.

When a LHS lookup fails, *Scope* will create a new variable in global *Scope* unless in "Strict Mode"  
Strict Mode would cause *Engine* to throw a `ReferenceError`.

When a variable is found in a RHS lookup, but you try to do something impossible with it, *Engine* throws a `TypeError`.
