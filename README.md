# Frontend Interview Cheatsheet

## JS theory

### Describe the data types available in Javascript?

There are 8 basic data types in JavaScript:

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

- Hoisting is a Javascript default behavior of moving functions, variables or classes declarations on top of the module/function level scope before executing the code.
- Function declaration are hoists while function expressions are not

```js
// This is function declaration
function foo() { ... }

// This is function expression
var bar = function () { ... }
```

- Variables hoisting
  - Variables declared with `var` hoisted to the top of its functional or global scope, but the initialization is not. If you try to use a variable before it is declared and initialized, it will have the value `undefined`.
  - Variablse declared with `let` and `const` hoisted, but they are not initialized with a default value. Accessing them before the actual declaration results in a `ReferenceError`. This is often referred to as the "temporal dead zone."

```js
console.log(a); // undefined, due to hoisting
var a = 5;

console.log(b); // ReferenceError: b is not defined
let b = 3;
```



### 



