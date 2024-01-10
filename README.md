# Frontend Interview Cheatsheet

> Frontend Interview Survival Cheatsheet

- [**JS theory**](#js-theory)
  * [What are the data types in JavaScript?](#what-are-the-data-types-in-javascript)
  * [What is the Hoisting?](#what-is-the-hoisting)
  * [What is the Closure?](#what-is-the-closure)
  * [What is the Event Loop?](#what-is-the-event-loop)
  * [What happens when you type URL in browser?](#what-happens-when-you-type-url-in-a-browser)
  * [How does a browser render a webpage?](#how-does-a-browser-render-a-webpage)
  * [Difference between var, let, const](#difference-between-var-let-const)
  
- [**JS problems**](#js-problems)
  
  - [Implement debounce decorator](#implement-debounce-decorator)
  - [Flatten the array](#flatten-the-array)
  - [Implement forEach](#implement-foreach)
  - [Implement Array.prototype.map](#implement-arrayprototypemap)
  - [Implement spy decorator](#implement-spy-decorator)
  - [Implement throttle](#implement-throttle)
  
- [**HTML & CSS**](#)
  
  - [How to center a Div element](#how-to-center-a-div-element)
  - [Difference between px, rem, em](#difference-between-px-rem-em-units)
  
    
  
    
  



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

### Implement debounce

Debounce decorator is a high-order function that delays the function execution by the given timer. You are required to implement it. 

Debouncing is particularly useful when you want to avoid the function being fired too frequently. A common use case is in search bars where you might want to wait for the user to finish typing before making an API call to fetch search results.

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

### Flatten the array

You are required to implement a custom `Array.prototype.flat` function. This function creates a new array with all sub-array elements concatenated into it recursively up to the specified depth.

**Example**

```js
// Input
const arr1 = [0, 1, 2, [3, 4]];

// Output
const res = flat(arr, 1) 
console.log(res) // [0, 1, 2, 3, 4]
```

**Recursive solution**

General idea:  

- Let's create a recursive function. 
- Let's write a base case for recursion: if depth is equal to 0, then just return given array
- Iterate over a given array and on every step of iteration check whether the current item is an array or no
  - If it's an array, then flat it with the spread operator and call the recursive function with the current item
  - If it's not an array, then just push it to `result` array
- Return result

**Code**

```js
function flat(arr, depth = 1) {
  if (depth === 0) return arr;
  const result = []
  for (const item of arr) {
    if (Array.isArray(item)) {
      result.push(...flat(item, depth - 1)) 
    } else {
      result.push(item);
    }
  }
  
  return result;
}

const test = [1, 2, [3, 4, [5, 6], 7, 8], 9];
console.log(flat(test, 2)); // [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

- Time complexity: `O(N)`
- Space complexity: `O(N)`



### Implement forEach

You are required to implement `Array.prototype.forEach` method, which calls the given callback function for each element in a given array.

**Example:**

```js
const arr = [1, 2, 3];
arr.forEach((item) => {
  console.log(item);
});
// Output: 1, 2, 3
```

**Solution**

Let's just iterate over a given array, and for every item, call the given callback function.

```js
Array.prototype.myForEach = function(cb) {
  for (let i = 0; i < this.length; i++) {
    cb(this[i], i, this);
  }        
};

const arr = [1, 2, 3, 4, 5];
arr.myForEach( (item) => {
  console.log(item); // 1, 2, 3, 4, 5
});
```

Where:

- `cb` is a callback function
- `this[i]` is a current item of an array
- `this` is a given array

**Complexity:**

- Time complexity: `O(N)`
- Space complexity: `O(1)`



### Implement Array.prototype.map

You are required to implement a custom `Array.prototype.map` method, which is used to create a new array by applying a given callback function to every element of an original array. This method is particularly useful for transforming data.

**Example:**

```js
const numbers = [1, 2, 3, 4, 5];

const squaredNums = numbers.map((number) => {
  return number * number;
});

console.log(squaredNums); // Output: [1, 4, 9, 16, 25]

```

**Solution**

- Start by extending the `Array.prototype` to add the `myMap` function. This makes `myMap` available to all instances of arrays in your code.
- Define the `myMap` function. The `myMap` function should take a callback function (`cb`) as its argument. This callback function will be applied to each element of the array.
- Inside `myMap`, initialize an empty array named `result`. This array will store the results of applying the callback function to each array element.
- Use a `for` loop to iterate over each element of the array (`this`). The `this` keyword refers to the array on which `myMap` is called.
  - In each iteration, apply the callback function to the current element, its index, and the entire array (`cb(this[i], i, this)`).
- Push the result of the callback function into the `result` array.
- After the loop completes, return the `result` array. This array contains the transformed elements.

```js
Array.prototype.myMap = function(cb) {
  let result = [];
  for (let i = 0; i < this.length; i++) {
    result.push(cb(this[i], i, this));
  }
  return result;
}

// Usage
const arr = [1, 2, 3, 4, 5]
const res = arr.myMap(item => {
  return item*2;
})

console.log(res); // 1, 4, 6, 8, 10
```

Where:

- `cb` is a callback function
- `this[i]` is a current item of an array
- `this` is a given array

**Complexity:**

- Time complexity: `O(N)`
- Space complexity: `O(N)`



### Implement spy decorator

The `Decorator` is a design pattern that allows you to modify the existing functionality of a function by wrapping it in another function. 

In the JavaScript testing framework Jest, a "spy" is typically used to monitor the behavior of a function: 

```js
spyOn(object, methodName)
```

You are required to implement Spy Decorator, which counts how many times the given function was called and with which arguments.

**Example**

```js
const obj = {
   counter: 1, 
   add(num) {
      this.counter += num
   }
}

const spy = spyOn(obj, 'add')

obj.add(1) // 2
obj.add(2) // 4

console.log(obj.counter) // 4
console.log(spy.calls) // [ [1], [2] ]

```

**Solution**

- Let's create a `spyOn` function which takes two arguments:
  - `obj` - the object containing the method to be spied on
  - `methodName` - a string representing the name of a method
- Let's initialize an array `calls`, which is used to store the arguments with which the spied method is called.
- Create `originMethod` which will be the reference to the original method from the object. This is necessary to maintain the original functionality of the method while adding the spy capability.
- Ensure that the specified `methodName` corresponds to a function in the object. If not, an error is thrown. This is a safeguard to prevent incorrect usage of the `spyOn` function.
- The original method on the object is then overridden with a new function. This new function does two things:
  - It records the arguments passed to the method call in the `calls` array.
  - It calls the original method using `apply` to ensure that the context (`this`) and arguments are passed correctly.
- Finally, the `spyOn` function returns an object containing the `calls` array. This allows the user of the function to inspect the arguments with which the spied method has been called.

**Code**

```js
function spyOn(obj, methodName) {
  const calls = [];

  const originMethod = obj[methodName];

  if (typeof originMethod !== "function") {
    throw new Error("not function");
  }

  obj[methodName] = function (...args) {
    calls.push(args);
    return originMethod.apply(this, args);
  };

  return {
    calls,
  };
}
```



### Implement throttle

Throttling is a technique that ensures that the function does not execute more than once in a specified time period.

This is useful for managing the performance of functions that could be triggered frequently. 

You are required to implement `throttle(fn, delay)` decorator function.

**Example:**

```js
function throttle(fn, delay) {
  // Your implementation
}

const print = () => {
  console.log("print");
}

const fn = throttle(print, 1000);

// fn function will be called once in a second on window resize
window.addEventListener("resize", fn);
```

**Solution**

```js
function throttle(fn, delay) {
  // Initialize a timer variable for managing the throttling
  let timer = null;

  // Return a new function that encapsulates the throttling logic
  return (...args) => {
    // if timer is already running, then just skip fn call
    if (timer) return;

    // Set a timer that delays the function execution
    timer = setTimeout(() => {
      // Execute the original function with the provided arguments
      fn(...args);
      
      // Clear the timer after the function executes and reset it to null
      clearTimeout(timer);
      timer = null;
    }, delay);
  };
}

const print = () => {
  console.log("print");
}

const fn = throttle(print, 1000);

// fn function will be called once in a second on window resize
window.addEventListener("resize", fn);
```



## HTML & CSS

### How to center a DIV element

How to center a DIV element horizontally and vertically? Name a different approaches. 

Let's say that we have the following HTML structure:

```html
<div class="parent">
  <div class="child">Centered Content</div>
</div>
```

**Approach 1: Using Flexbox**

If the parent container of your `div` is a flex container, you can center the `div` both horizontally and vertically with the following CSS:

```css
.parent {
  display: flex;
  justify-content: center; /* align horizontal */
  align-items: center;     /* align vertical */
}
```

**Approach 2: Using Grid**

Similar to Flexbox, but with CSS Grid.

`place-items` is a shorthand for `align-items` and `justify-items`

```css
.parent {
  display: grid;
  place-items: center;
}
```

**Approach 3: Using Margin Auto**

If you want to center a `div` horizontally, and it has a fixed width, you can use:

```css
.child {
  margin: 0 auto; /* auto margin on left and right */
  width: 50%;    /* or any fixed width */
}
```

**Approach 4: Using Absolute Positioning**

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

### Difference between px, rem, em units

There are two types of units in CSS:

- Absolute units: px
- Relative units: em, rem, %, vh

Absolute units don't change, while relative units are flexible and change based on context.

Let's dive into the most popular of them: `px`, ` em`, and `rem`.

**Pixels (px)**

"Pixels (PX) are a common unit of measurement in web design. They are fixed and useful for maintaining consistent sizing of certain elements. However, they can pose problems for responsive sites."

**Relative EM (em)**

The `em` unit is relative to the font size of its closest parent element.

**Root EM (rem)**

REM is similar to EM, but always relative to the root element (HTML tag). 

Relative units like %, EM, and REM are better for responsive design and accessibility standards.

### Difference between `<script>`, `<script async>`, `<script defer>`

In HTML, the `<script>` tag is used to embed a JavaScript script in your webpage. 

You can modify the behavior of this tag by using the async and defer attributes.

**`<script>`**

Script without any attributes is fetched and executed immediately, blocking the parsing of the rest of the HTML document until the script is loaded and executed.

**`<script async>`**

The script will be fetched in parallel to the HTML parsing and executed as soon as it’s downloaded. This does not block the HTML parsing.

It's often used for analytics or ad network-scripts.

**`<script defer>`**

The defer attribute fetches the script parallel to HTML parsing but delays its execution until after HTML parsing is complete. This ensures that the script and DOM work as intended.
