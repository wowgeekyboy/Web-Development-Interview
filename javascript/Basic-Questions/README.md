# Comprehensive JavaScript Interview Questions and Answers

## Basic Questions

### 1. What is JavaScript?

JavaScript is a high-level, interpreted programming language primarily used for creating interactive and dynamic content on web pages. It's a core technology of the World Wide Web, alongside HTML and CSS.

**Key features:**
- Client-side scripting language (runs in the browser)
- Server-side capabilities (Node.js)
- Supports object-oriented, imperative, and functional programming paradigms
- Dynamic typing and first-class functions
- Prototype-based object-oriented programming

**Example:**
```javascript
// A simple JavaScript function
function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet("World"); // Output: Hello, World!
```

### 2. What are the different data types in JavaScript?

JavaScript has several built-in data types:

1. **Primitive types:**
   - Number: Represents both integer and floating-point numbers
   - String: Represents textual data
   - Boolean: Represents true or false
   - Undefined: Represents a variable that has been declared but not assigned a value
   - Null: Represents a deliberate non-value or absence of any object value
   - Symbol (ES6): Represents a unique identifier
   - BigInt (ES11): Represents integers larger than 2^53 - 1

2. **Object type:**
   - Object: Represents a collection of related data or functionality

**Example:**
```javascript
let num = 42;               // Number
let str = "Hello";          // String
let bool = true;            // Boolean
let undef = undefined;      // Undefined
let n = null;               // Null
let sym = Symbol("unique"); // Symbol
let bigInt = 1234567890123456789012345678901234567890n; // BigInt

let obj = {                 // Object
  name: "John",
  age: 30
};

console.log(typeof num, typeof str, typeof bool, typeof undef, typeof n, typeof sym, typeof bigInt, typeof obj);
// Output: number string boolean undefined object symbol bigint object
```

### 3. What is the difference between `null` and `undefined`?

While both `null` and `undefined` represent the absence of a value, they have distinct meanings:

**Undefined:**
- Represents a variable that has been declared but hasn't been assigned a value
- Is the default value of function parameters and uninitialized variables
- Is a primitive value

**Null:**
- Represents an intentional absence of any object value
- Must be explicitly assigned
- Is an object (which is often considered a bug in JavaScript)

**Example:**
```javascript
let a;
console.log(a);        // Output: undefined

let b = null;
console.log(b);        // Output: null

console.log(typeof a); // Output: undefined
console.log(typeof b); // Output: object (this is a known quirk in JavaScript)

function greet(name) {
  console.log(`Hello, ${name}!`);
}

greet();               // Output: Hello, undefined!

let obj = {
  notHere: null
};

console.log(obj.notHere);     // Output: null
console.log(obj.nonExistent); // Output: undefined
```

### 4. What is hoisting in JavaScript?

Hoisting is a behavior in JavaScript where variable and function declarations are moved to the top of their respective scopes during the compilation phase, before the code is executed.

**Key points:**
- Only declarations are hoisted, not initializations
- Function declarations are hoisted completely
- Variables declared with `var` are hoisted and initialized with `undefined`
- Variables declared with `let` and `const` are hoisted but not initialized (Temporal Dead Zone)

**Example:**
```javascript
console.log(x); // Output: undefined
var x = 5;

// The above is interpreted as:
var x;
console.log(x);
x = 5;

// Function hoisting
greet("World"); // Output: Hello, World!

function greet(name) {
  console.log(`Hello, ${name}!`);
}

// let and const hoisting (Temporal Dead Zone)
console.log(y); // Throws ReferenceError
let y = 10;
```

**Diagram: Hoisting Process**
```
┌─────────────────────────────────┐
│ Memory allocation               │
│                                 │
│ ┌───────────────────────────┐   │
│ │ Function declarations     │   │
│ │ Variable declarations     │   │
│ └───────────────────────────┘   │
│               ▼                 │
│ ┌───────────────────────────┐   │
│ │ Code execution            │   │
│ └───────────────────────────┘   │
└─────────────────────────────────┘
```

### 5. What are closures in JavaScript?

A closure is a function that has access to variables in its outer (enclosing) lexical scope, even after the outer function has returned. It's a powerful feature that allows for data privacy and the creation of function factories.

**Key characteristics:**
- Closures have access to variables in their own scope, in outer functions, and global variables
- The inner function preserves the scope chain of the enclosing function at the time the closure is created
- Closures store references to the outer function's variables, not copies

**Example:**
```javascript
function createCounter() {
  let count = 0;
  return function() {
    count++;
    console.log(count);
  };
}

const counter = createCounter();
counter(); // Output: 1
counter(); // Output: 2
counter(); // Output: 3
```

**Diagram: Closure Concept**
```
┌───────────────────────────┐
│ createCounter             │
│ ┌───────────────────────┐ │
│ │ count: 0              │ │
│ │ ┌───────────────────┐ │ │
│ │ │ returned function │ │ │
│ │ │ (closure)         │ │ │
│ │ └───────────────────┘ │ │
│ └───────────────────────┘ │
└───────────────────────────┘
           ▼
┌───────────────────────────┐
│ counter                   │
│ (has access to count)     │
└───────────────────────────┘
```

### 6. What is a callback function in JavaScript?

A callback function is a function passed as an argument to another function, which is then invoked inside the outer function to complete some kind of routine or action. Callbacks are fundamental to asynchronous programming in JavaScript.

**Key points:**
- Callbacks allow asynchronous code execution
- They help in handling the results of asynchronous operations
- Callbacks can lead to "callback hell" if not managed properly

**Example:**
```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "John Doe" };
    callback(data);
  }, 1000);
}

function processData(data) {
  console.log("Data processed:", data);
}

fetchData(processData);
// Output after 1 second: Data processed: { id: 1, name: "John Doe" }
```

**Diagram: Callback Execution**
```
┌────────────────┐       ┌────────────────┐
│  fetchData     │       │   processData  │
│  ┌──────────┐  │       │                │
│  │ setTimeout│  │       │                │
│  └──────────┘  │       │                │
│       │        │       │                │
│       ▼        │       │                │
│  ┌──────────┐  │       │                │
│  │ callback ├──┼───────┼───────────────▶│
│  └──────────┘  │       │                │
└────────────────┘       └────────────────┘
```

### 7. What are promises in JavaScript?

Promises are objects representing the eventual completion or failure of an asynchronous operation. They provide a cleaner alternative to callbacks for handling asynchronous code.

**States of a Promise:**
1. Pending: Initial state, neither fulfilled nor rejected
2. Fulfilled: The operation completed successfully
3. Rejected: The operation failed

**Key methods:**
- `then()`: Handles the fulfillment of a promise
- `catch()`: Handles the rejection of a promise
- `finally()`: Executes regardless of the promise's outcome

**Example:**
```javascript
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const success = Math.random() > 0.5;
      if (success) {
        resolve({ id: 1, name: "John Doe" });
      } else {
        reject("Error fetching data");
      }
    }, 1000);
  });
}

fetchData()
  .then(data => console.log("Data received:", data))
  .catch(error => console.error("Error:", error))
  .finally(() => console.log("Operation completed"));
```

**Diagram: Promise States**
```
┌─────────────┐
│   Pending   │
└─────┬───────┘
      │
      ▼
┌─────────────┐     ┌─────────────┐
│  Fulfilled  │ or  │  Rejected   │
└─────────────┘     └─────────────┘
```

### 8. What is the purpose of the `setTimeout()` function?

The `setTimeout()` function is used to schedule the execution of a function after a specified delay in milliseconds. It's part of the Web APIs in browsers and is commonly used for delaying operations or creating time-based animations.

**Key points:**
- `setTimeout()` is asynchronous and non-blocking
- It returns a unique identifier that can be used to cancel the timeout using `clearTimeout()`
- The minimum delay is not guaranteed due to the single-threaded nature of JavaScript

**Syntax:**
```javascript
setTimeout(function, delay, param1, param2, ...)
```

**Example:**
```javascript
console.log("Start");

setTimeout(() => {
  console.log("This message appears after 2 seconds");
}, 2000);

console.log("End");

// Output:
// Start
// End
// This message appears after 2 seconds (after 2 seconds)
```

**Diagram: setTimeout Execution**
```
┌─────────────────────────────────────────────────┐
│ Call Stack                                      │
├─────────────────────────────────────────────────┤
│ 1. console.log("Start")                         │
│ 2. setTimeout(callback, 2000)                   │
│ 3. console.log("End")                           │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│ Web API / Timer                                 │
│ (waits for 2 seconds)                           │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│ Callback Queue                                  │
│ (callback function waiting to be executed)      │
└─────────────────────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────┐
│ Event Loop                                      │
│ (moves callback to Call Stack when it's empty)  │
└─────────────────────────────────────────────────┘
```

### 9. How can you check if an array includes a certain value?

There are several ways to check if an array includes a certain value in JavaScript:

1. **Using the `includes()` method (ES6+):**
   This method returns `true` if the array contains the specified element, and `false` otherwise.

   ```javascript
   const fruits = ['apple', 'banana', 'orange'];
   console.log(fruits.includes('banana')); // Output: true
   console.log(fruits.includes('grape'));  // Output: false
   ```

2. **Using the `indexOf()` method:**
   This method returns the index of the first occurrence of the specified element, or -1 if it's not found.

   ```javascript
   const fruits = ['apple', 'banana', 'orange'];
   console.log(fruits.indexOf('banana') !== -1); // Output: true
   console.log(fruits.indexOf('grape') !== -1);  // Output: false
   ```

3. **Using the `some()` method:**
   This method tests whether at least one element in the array passes the test implemented by the provided function.

   ```javascript
   const fruits = ['apple', 'banana', 'orange'];
   console.log(fruits.some(fruit => fruit === 'banana')); // Output: true
   console.log(fruits.some(fruit => fruit === 'grape'));  // Output: false
   ```

4. **Using a for loop (works for older browsers):**
   This method manually iterates through the array to check for the value.

   ```javascript
   function includesValue(arr, value) {
     for (let i = 0; i < arr.length; i++) {
       if (arr[i] === value) {
         return true;
       }
     }
     return false;
   }

   const fruits = ['apple', 'banana', 'orange'];
   console.log(includesValue(fruits, 'banana')); // Output: true
   console.log(includesValue(fruits, 'grape'));  // Output: false
   ```

The `includes()` method is generally the most straightforward and readable option for modern JavaScript environments.

### 10. How can you remove duplicates from an array?

There are several ways to remove duplicates from an array in JavaScript:

1. **Using Set (ES6+):**
   This is the most concise method, leveraging the unique property of Set objects.

   ```javascript
   const numbers = [1, 2, 2, 3, 4, 4, 5];
   const uniqueNumbers = [...new Set(numbers)];
   console.log(uniqueNumbers); // Output: [1, 2, 3, 4, 5]
   ```

2. **Using filter() method:**
   This method creates a new array with unique elements by checking the index of each element.

   ```javascript
   const numbers = [1, 2, 2, 3, 4, 4, 5];
   const uniqueNumbers = numbers.filter((value, index, self) => 
     self.indexOf(value) === index
   );
   console.log(uniqueNumbers); // Output: [1, 2, 3, 4, 5]
   ```

3. **Using reduce() method:**
   This method builds a new array with unique elements by accumulating unique values.

   ```javascript
   const numbers = [1, 2, 2, 3, 4, 4, 5];
   const uniqueNumbers = numbers.reduce((unique, item) => 
     unique.includes(item) ? unique : [...unique, item], []
   );
   console.log(uniqueNumbers); // Output: [1, 2, 3, 4, 5]
   ```

4. **Using forEach() with an object:**
   This method uses an object to keep track of unique values.

   ```javascript
   const numbers = [1, 2, 2, 3, 4, 4, 5];
   const uniqueNumbers = [];
   const seen = {};
   numbers.forEach(num => {
     if (!seen[num]) {
       seen[num] = true;
       uniqueNumbers.push(num);
     }
   });
   console.log(uniqueNumbers); 
   ```
