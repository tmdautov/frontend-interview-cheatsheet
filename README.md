# Frontend Interview Cheatsheet

> Frontend Interview Survival Cheatsheet

- [**JS theory**](#js-theory)
  * [What are the data types in JavaScript?](#what-are-the-data-types-in-javascript)
  * [What is the Hoisting?](#what-is-the-hoisting)
  * [What is the Closure?](#what-is-the-closure)
  * [What is the Event Loop?](#what-is-the-event-loop)
  * [What happens when you type URL in browser?](#what-happens-when-you-type-url-in-a-browser)
  * [How does a browser render a webpage?](#how-does-a-browser-render-a-webpage)



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

### What is the Event Loop?

The event loop is a single-threaded loop that monitors the call stack and the callback queue. If the call stack is empty, the event loop will take the first event from the queue and will push it to the call stack.

This event loop mechanism allows JavaScript to perform non-blocking I/O operations despite the fact that JavaScript is single-threaded.

The event loop has the following building blocks:

- `Call Stack`. The Call Stack is a data structure in Javascript used to keep track of the function calls in a program. Call Stack follows LIFO principle, which means that the last function that gets pushed onto the stack is the first to be popped off when its execution completes.
- `Callback Queue`. Callback queue is where callbacks from asynchronous operations are placed once they are ready to be executed.
- `Memory Heap`. This is where memory allocation happens in JavaScript. When you create variables and objects in your JavaScript code, they are stored in the memory heap. This area of memory is not structured and is used for the dynamic allocation, meaning that variables and objects can be allocated and deallocated in any order.
- `Web API`. The Web APIs are provided by the web browser that gives additional functionality to the V8 engine. The Web API calls are added to the Web API Container from the Call Stack. These Web API calls remain inside the Web API Container until an action is triggered. 

This is a simplified version of Event Loop implementation:

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

### What happens when you type URL in a Browser

There are 6 general steps in this process:

1. DNS lookup:

   - `DNS lookup in Cache`. The browser uses DNS lookup to get the IP address for the domain. Cached data at different levels makes the process faster.

   - `Recursive DNS lookup in Server` If the IP address cannot be found at any of the caches, the browser goes to DNS servers to do a recursive DNS lookup until the IP address is found
2. `TCP handshake.` The browser establishes a TCP connection with the Server.
3. `TLS Negotiation`. Determines which cipher will be used to encrypt the communication, verifies the Server, and establishes that a secure connection is in place before beginning the actual transfer of data
4. `Request to the Server`. The Client sends an HTTP request to the Server.
5. `Server response to the Client`. The Server processes the request and sends back the response for a successful response (the status code is 200).
6. `Rendering webpage`. The browser renders the HTML content.

### How does a browser render a webpage?

There are 6 general steps in this process:

1. `Construct DOM tree`: a tree of nodes corresponding to `HTML` elements
2. `Construct CSSOM tree`: a tree of nodes corresponding to `CSS` selectors
3. `Construct Render tree`: The browser traverses the DOM tree and matches every element with the appropriate style from the `CSSOM` tree.
4. `Layout phase`. geometry phase where browsers calculate the position of every element
5. `Painting phase`. The browser uses a render tree to paint or display pixels currently within your viewport
6. `Repainting phase`: if we change the DOM tree, the browser must repaint the affected regions

### Difference between `var`, `let`, `const`

In JavaScript, `var`, `let`, and `const` are all used to declare variables, but they differ in their scope, hoisting, and reassignment capabilities. There are key differences:

- Difference in scope:
  - `var` is function or globally scoped, 
  -  `let` and `const` are block-scoped
- Difference in redeclaration and reassignment:
  - `var` allows redeclaration and reassignment. 
  - `let` allows reassignment but not redeclaration in the same scope. 
  - `const` does not allow reassignment or redeclaration.
- Difference in hoisting:
  - Variables declared with `var` are hoisted and can be accessed before their declaration (with `undefined` value)
  - Variables declared with `let` and `const` are also hoisted to the top of their block, but they should not be initialized. Accessing them before the declaration results in a `ReferenceError` (Temporal Dead Zone).

## JS Problems

### Implement debounce decorator

Debounce decorator is a high-order function that delays the function execution by the given timer. You are required to implement it. 

Debouncing is particularly useful in situations where you want to avoid the function being fired too frequently. A common use case is in search bars where you might want to wait for the user to finish typing before making an API call to fetch search results.

```js
const debounce = (fn, ms) => {
   // ... your implementation
}
```

**Example**

```js
const print = (n) => {
  console.log(n);
}

const fn = debounce(print, 1000);
fn(1); // ignore
fn(2); // ignore
fn(3); // prints 3
```

**Solution**

```js
const debounce = (fn, ms) => {
  // timeoutID is used to store the ID of the setTimeout, allowing it to be cleared later.
  let timeoutID = null;
  
  // Returns a new function that has debouncing functionality.
  return (...args) => {
    // Clears the current timeout, if any, to reset the wait time for the function call.
    clearTimeout(timeoutID);    
    
    // Sets a new timeout. The original function fn will be called after the specified ms delay.
    timeoutID = setTimeout(() => {
      fn.call(this, ...args);
    }, ms);      
  }
} 

// Usage
window.addEventListener('resize', debounce(() => {
  // Will be printed once in a second
  console.log('Resizing...');
}, 1000));
```

