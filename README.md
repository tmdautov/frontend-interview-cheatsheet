# Frontend Interview Cheatsheet

- [**JS theory**](#js-theory)
  * [What are the data types in JavaScript?t?](#describe-the-data-types-available-in-javascript)
  * [What is the Hoisting?](#what-is-the-hoisting)
  * [What is the Closure?](#what-is-the-closure)



## JS theory

### What are the data types in JavaScript?

> There are 8 basic data types in JavaScript

- **Primitive (7 types)**
  - `String`
  - `Number`
  - `Boolean`
  - `BigInt`
  - `null`
  - `undefined`
  - `Symbol`
- **Non-primitive (1 type)**
  - `Object`: This is the base type for more complex structures like Arrays, Functions, and other objects

### What is the Hoisting?

> Hoisting is a Javascript default behavior of moving functions, variables or classes declarations on top of the module/function level scope before executing the code.

- Function declarations are hoists, while function expressions are not

```js
// This is function declaration
function foo() { ... }

// This is function expression
var bar = function () { ... }
```

- Variables hoisting
  - Variables declared with `var` hoisted to the top of its functional or global scope, but the initialization is not. If you try to use a variable before it is declared and initialized, it will have the value `undefined`.
  - Variables declared with `let` and `const` hoisted, but they are not initialized with a default value. Accessing them before the actual declaration results in a `ReferenceError`. This is often referred to as the "temporal dead zone."

```js
console.log(a); // undefined, due to hoisting
var a = 5;

console.log(b); // ReferenceError: b is not defined
let b = 3;
```

### What is the Closure?

>  Closure is a function that has access to its own scope, the scope of the outer function and the global scope.

- Closures can be used to emulate private properties and methods.
- Lexical scope
  - Global scope refers to the context in which variables and functions are accessible from any part of the code.
  - Block scope was introduced with ES6 with `let` and `const`. Previously Javascript only had Global and Function Scope. Variables declared inside block scope can not be accessed from outside of the block.
  - Every function create his own scope. Variable declared inside a function can only be accessible within that function and cannot be used outside that function.


```
```





