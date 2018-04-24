# You Don't Know JS: Scope & Closures
# Chapter 1: What is Scope?
[link](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch1.md)

Where and how do *Scope* rules get set?

## Compiler Theory

Javascript falls under the general category of "dynamic" or "interpreted" languages, but is in fact a compiled language. The results of compilation are often not portable among various distributed systems so it's not compiled in advance.

In a traditional compiled-language process, a program will undergo typically three steps *before* it is executed, roughly called "compilation":

1. **Tokenizing/Lexing:** breaking up a string of characters into meaningful (to the language) chunks, called tokens. For instance, consider the program: `var a = 2;`. This program would likely be broken up into the following tokens: `var`, `a`, `=`, `2`, and `;`. Whitespace may or may not be persisted as a token, depending on whether it's meaningful or not.

    **Note:** The difference between tokenizing and lexing is subtle and academic, but it centers on whether or not these tokens are identified in a *stateless* or *stateful* way. Put simply, if the tokenizer were to invoke stateful parsing rules to figure out whether `a` should be considered a distinct token or just part of another token, *that* would be **lexing**.

2. **Parsing:** taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an "AST" (<b>A</b>bstract <b>S</b>yntax <b>T</b>ree).

    The tree for `var a = 2;` might start with a top-level node called `VariableDeclaration`, with a child node called `Identifier` (whose value is `a`), and another child called `AssignmentExpression` which itself has a child called `NumericLiteral` (whose value is `2`).

3. **Code-Generation:** the process of taking an AST and turning it into executable code. This part varies greatly depending on the language, the platform it's targeting, etc.

    So, rather than get mired in details, we'll just handwave and say that there's a way to take our above described AST for `var a = 2;` and turn it into a set of machine instructions to actually *create* a variable called `a` (including reserving memory, etc.), and then store a value into `a`.

    **Note:** The details of how the engine manages system resources are deeper than we will dig, so we'll just take it for granted that the engine is able to create and store variables as needed.

## Understanding Scope

### The Cast

1. *Engine*: responsible for start-to-finish compilation and execution of a JavaScript program.

2. *Compiler*: handles all the dirty work of parsing and code-generation.

3. *Scope*: collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

### Back & Forth

*Engine* sees `var a = 2;` as two distinct statements, one which *Compiler* will handle during compilation, and one which *Engine* will handle during execution.

The first thing *Compiler* will do with this program is perform lexing to break it down into tokens.

*Compiler* will then proceed as:

1. Encountering `var a`, *Compiler* asks *Scope* to see if a variable `a` already exists for that particular scope collection. If so, *Compiler* ignores this declaration and moves on. Otherwise, *Compiler* asks *Scope* to declare a new variable called `a` for that scope collection.

2. *Compiler* then produces code for *Engine* to later execute, to handle the `a = 2` assignment. The code *Engine* runs will first ask *Scope* if there is a variable called `a` accessible in the current scope collection. If so, *Engine* uses that variable. If not, *Engine* looks *elsewhere*.

If *Engine* eventually finds a variable, it assigns the value `2` to it. If not, *Engine* will throw an error!

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

foo( 2 ); // foo is RHS

function foo(a) { // LHS (assigning 2 to a)
	console.log( a ); // RHS
}

function bar(){ /* ... */ } // no lookup, declaration and value definition are handled during code generation

var b = 2 // this is still LHS
```
