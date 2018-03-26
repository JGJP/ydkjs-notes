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

When *Engine* executes the code that *Compiler* produced for step (2), it has to look-up the variable `a` to see if it has been declared, and this look-up is consulting *Scope*. But the type of look-up *Engine* performs affects the outcome of the look-up.

In this case, it is said that *Engine* would be performing an "LHS" (Left-hand Side) look-up for the variable `a`. The other type of look-up is called "RHS" (Right-hand Side).

An LHS look-up is done when a variable appears on the left-hand side of an assignment operation, and an RHS look-up is done when a variable appears on the right-hand side of an assignment operation.

To be more precise, an RHS look-up is indistinguishable, for our purposes, from simply a look-up of the value of some variable, whereas the LHS look-up is trying to find the variable container itself, so that it can assign. In this way, RHS doesn't *really* mean "right-hand side of an assignment" per se, it just, more accurately, means "not left-hand side".

Yyou could also think "RHS" instead means "retrieve his/her source (value)", implying that RHS means "go get the value of...".

```js
console.log( a );
```

The reference to `a` is an RHS reference, because nothing is being assigned to `a` here.

```js
a = 2;
```

The reference to `a` here is an LHS reference, because it doesn't matter what the current value is, we simply want to find the variable as a target for the `= 2` assignment operation.

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

The last line that invokes `foo(..)` as a function call requires an RHS reference to `foo`, meaning, "go look-up the value of `foo`, and give it to me." Moreover, `(..)` means the value of `foo` should be executed, so it has to be a function.

There is an implied `a = 2` when the value `2` is passed as an argument to the `foo(..)` function, in which case the `2` value is **assigned** to the parameter `a` -> LHS lookup.

There's also an RHS reference for the value of `a`, and that resulting value is passed to `console.log(..)`.  
`console.log(..)` is an RHS look-up for the `console` object, then a property-resolution occurs to see if it has a method called `log`.

**Note:** You might be tempted to conceptualize the function declaration `function foo(a) {...` as a normal variable declaration and assignment, such as `var foo` and `foo = function(a){...`. In so doing, it would be tempting to think of this function declaration as involving an LHS look-up.

However, the subtle but important difference is that *Compiler* handles both the declaration and the value definition during code-generation, such that when *Engine* is executing code, there's no processing necessary to "assign" a function value to `foo`. Thus, it's not really appropriate to think of a function declaration as an LHS look-up assignment in the way we're discussing them here.