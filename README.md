# Frontend Interview Cheatsheet

- [**JS theory**](#js-theory)
  * [What are the data types in JavaScript?t?](#what-are-the-data-types-in-javascript)
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

- Closure is a function that has access to its own scope, the scope of the outer function, and the global scope.

- Closures can be used to emulate private properties and methods.
- Every JavaScript function creates a new `lexical environment`. This environment has a reference to its outer environment, forming a scope chain
- The scope chain is a process of how Javascript looks for variables. When code needs to access a variable, JavaScript looks at the current scope. If not found, it moves up the scope chain to the outer scope and continues until it reaches the global scope. If the variable is still not found, a reference error is thrown.

**Example: Counter**

```js
function getCounter() {
  let counter = 0;
  return function() {
    return counter++;
  }
}

let count = getCounter();
console.log(count()); // 0
console.log(count()); // 1
console.log(count()); // 2
```

### What is Event Loop?

The event loop is a single-threaded loop that monitors the call stack and the callback queue. If the call stack is empty, the event loop will take the first event from the queue and will push it to the call stack.

This event loop mechanism allows JavaScript to perform non-blocking I/O operations despite the fact that JavaScript is single-threaded.

Event loop has the following building blocks:

- `Call Stack`. The Call Stack is a data structure in Javascript used to keep track of the function calls in a program. Call Stack follows LIFO principle, which means that the last function that gets pushed onto the stack is the first to be popped off when its execution completes.
- `Callback Queue`. Callback queue is where callbacks from asynchronous operations are placed once they are ready to be executed.
- `Memory Heap`. This is where memory allocation happens in JavaScript. When you create variables and objects in your JavaScript code, they are stored in the memory heap. This area of memory is not structured and is used for the dynamic allocation, meaning that variables and objects can be allocated and deallocated in any order.
- `Web API`. The Web APIs are provided by the web browser that gives additional functionality to the V8 engine. The Web API calls are added to the Web API Container from the Call Stack. These Web API calls remain inside the Web API Container until an action is triggered. 

This is simplified version of Event Loop implementation:

```js
while (true) {
  if (!isCallStackEmpty) {
    continue;
  }

  for (const task of eventLoop.microtasks) {
    task.execute();
  }
  eventLoop.microtasks = [];

  const task = eventLoop.nextMacrotask();
  if (task) {
    task.execute();
  }

  if (eventLoop.needRender()) {
    eventLoop.render();
  }
}
```





