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

### Input

```js
age = prompt( "Please tell me your age:" );
console.log( age );
```

### Operators

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

### Values & Types

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