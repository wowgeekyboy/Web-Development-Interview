# Comprehensive JavaScript Interview Questions and Answers

## Intermediate Questions

### 1. What is the `this` keyword in JavaScript?

The `this` keyword in JavaScript refers to the object that is currently executing the function. Its value is determined by how a function is called.

**Key points:**
- The value of `this` can change depending on the context
- In the global scope, `this` refers to the global object (window in browsers, global in Node.js)
- In a method, `this` refers to the object the method was called on
- In a constructor function, `this` refers to the newly created instance
- The value of `this` can be explicitly set using `call()`, `apply()`, or `bind()`

**Examples:**

1. Global context:
```javascript
console.log(this === window); // true (in a browser)
```

2. Object method:
```javascript
const obj = {
  name: "John",
  greet: function() {
    console.log(`Hello, ${this.name}!`);
  }
};
obj.greet(); // Output: Hello, John!
```

3. Constructor function:
```javascript
function Person(name) {
  this.name = name;
}
const john = new Person("John");
console.log(john.name); // Output: John
```

4. Explicit binding:
```javascript
function greet() {
  console.log(`Hello, ${this.name}!`);
}
const person = { name: "Alice" };
greet.call(person); // Output: Hello, Alice!
```

5. Arrow functions:
```javascript
const obj = {
  name: "John",
  greet: () => {
    console.log(`Hello, ${this.name}!`);
  }
};
obj.greet(); // Output: Hello, undefined!
// Arrow functions do not have their own `this` binding
```

**Diagram: `this` in Different Contexts**
```
┌─────────────────────┐
│ Global Scope        │
│  this → global obj  │
│  ┌─────────────────┐│
│  │ Function        ││
│  │  this → dynamic ││
│  │  ┌─────────────┐││
│  │  │Object Method│││
│  │  │ this → obj  │││
│  │  └─────────────┘││
│  └─────────────────┘│
└─────────────────────┘
```

### 2. Explain the concept of prototypal inheritance.

Prototypal inheritance is a fundamental concept in JavaScript where objects can inherit properties and methods from other objects through a prototype chain.

**Key points:**
- Every object in JavaScript has a hidden `[[Prototype]]` property (accessible via `__proto__`)
- Objects can inherit properties and methods from their prototype
- The prototype chain ends with `Object.prototype`, whose prototype is `null`
- `Object.create()` can be used to create an object with a specific prototype

**Example:**
```javascript
// Constructor function
function Animal(name) {
  this.name = name;
}

// Adding a method to the prototype
Animal.prototype.speak = function() {
  console.log(`${this.name} makes a sound.`);
};

// Creating a new object
const dog = new Animal("Rex");
dog.speak(); // Output: Rex makes a sound.

// Checking the prototype chain
console.log(dog.__proto__ === Animal.prototype); // true
console.log(Animal.prototype.__proto__ === Object.prototype); // true
console.log(Object.prototype.__proto__ === null); // true
```

**Diagram: Prototype Chain**
```
┌─────────┐    ┌──────────────────┐    ┌───────────────────┐    ┌──────┐
│   dog   │───▶│ Animal.prototype │───▶│ Object.prototype  │───▶│ null │
└─────────┘    └──────────────────┘    └───────────────────┘    └──────┘
```

### 3. What are arrow functions and how do they differ from regular functions?

Arrow functions, introduced in ES6, provide a more concise syntax for writing function expressions. They differ from regular functions in several important ways.

**Key differences:**
1. Syntax: Arrow functions have a shorter syntax
2. `this` binding: Arrow functions do not have their own `this`
3. No `arguments` object: Arrow functions don't have the `arguments` object
4. Cannot be used as constructors: Arrow functions cannot be used with the `new` keyword
5. No `prototype` property: Arrow functions don't have a `prototype` property

**Examples:**

1. Syntax:
```javascript
// Regular function
function add(a, b) {
  return a + b;
}

// Arrow function
const addArrow = (a, b) => a + b;
```

2. `this` binding:
```javascript
const obj = {
  name: "John",
  greet: function() {
    setTimeout(function() {
      console.log(`Hello, ${this.name}!`); // `this` is not bound to obj
    }, 1000);
  },
  greetArrow: function() {
    setTimeout(() => {
      console.log(`Hello, ${this.name}!`); // `this` is bound to obj
    }, 1000);
  }
};

obj.greet(); // Output: Hello, undefined!
obj.greetArrow(); // Output: Hello, John!
```

3. No `arguments` object:
```javascript
function regular() {
  console.log(arguments);
}

const arrow = () => {
  console.log(arguments); // ReferenceError: arguments is not defined
};

regular(1, 2, 3); // Output: [1, 2, 3]
arrow(1, 2, 3); // Throws an error
```

4. Cannot be used as constructors:
```javascript
const Person = (name) => {
  this.name = name;
};

const john = new Person("John"); // TypeError: Person is not a constructor
```

**Diagram: Arrow vs Regular Functions**
```
┌─────────────────────────┐  ┌─────────────────────────┐
│    Regular Function     │  │     Arrow Function      │
├─────────────────────────┤  ├─────────────────────────┤
│ - Own `this` binding    │  │ - No own `this` binding │
│ - Has `arguments` object│  │ - No `arguments` object │
│ - Can be constructor    │  │ - Cannot be constructor │
│ - Has `prototype`       │  │ - No `prototype`        │
└─────────────────────────┘  └─────────────────────────┘
```

### 4. What is the purpose of `async` and `await`?

`async` and `await` are features introduced in ES2017 to simplify working with Promises and asynchronous code. They provide a more synchronous-looking way to write asynchronous code.

**Key points:**
- `async` functions always return a Promise
- `await` can only be used inside an `async` function
- `await` pauses the execution of the `async` function until the Promise is resolved
- Error handling can be done using try/catch blocks

**Example:**
```javascript
function fetchUser(id) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (id === 1) {
        resolve({ id: 1, name: "John Doe" });
      } else {
        reject("User not found");
      }
    }, 1000);
  });
}

async function getUser(id) {
  try {
    const user = await fetchUser(id);
    console.log("User:", user);
  } catch (error) {
    console.error("Error:", error);
  }
}

getUser(1); // Output after 1 second: User: { id: 1, name: "John Doe" }
getUser(2); // Output after 1 second: Error: User not found
```

**Diagram: async/await Flow**
```
┌─────────────────┐
│ async function  │
│  ┌─────────────┐│
│  │ await       ││
│  │ (pauses)    ││
│  └──────┬──────┘│
│         │       │
│  ┌──────▼──────┐│
│  │ continue    ││
│  │ execution   ││
│  └─────────────┘│
└─────────────────┘
```

### 5. Explain the JavaScript event loop.

The event loop is a fundamental concept in JavaScript that enables non-blocking, asynchronous execution. It continuously checks the call stack and the callback queue, moving callbacks to the call stack when it's empty.

**Key components:**
1. Call Stack: Where synchronous code execution happens
2. Web APIs: Where asynchronous operations are offloaded (in browsers)
3. Callback Queue: Where completed asynchronous operations wait
4. Microtask Queue: Higher priority queue for Promises and mutation observers
5. Event Loop: Checks if the call stack is empty and moves callbacks

**Example:**
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');

// Output:
// Start
// End
// Promise
// Timeout
```

**Diagram: Event Loop**
```
┌───────────────┐     ┌───────────────┐
│   Call Stack  │     │   Web APIs    │
│               │     │ (Async ops)   │
└───────┬───────┘     └───────┬───────┘
        │                     │
        │                     ▼
        │             ┌───────────────┐
        │             │ Callback Queue│
        │             └───────┬───────┘
        │                     │
        │             ┌───────────────┐
        │             │Microtask Queue│
        │             └───────┬───────┘
        │                     │
        └─────────────────────┘
               Event Loop
```

### 6. What are template literals?

Template literals, introduced in ES6, provide an easier way to create multiline strings and perform string interpolation.

**Key features:**
- Enclosed by backticks (`) instead of single or double quotes
- Can span multiple lines without explicit line breaks
- Allow embedded expressions using `${expression}`
- Support tagged templates for custom string parsing

**Examples:**

1. Basic usage:
```javascript
const name = "John";
const greeting = `Hello, ${name}!`;
console.log(greeting); // Output: Hello, John!
```

2. Multiline strings:
```javascript
const multiline = `
  This is a
  multiline
  string.
`;
console.log(multiline);
// Output:
//   This is a
//   multiline
//   string.
```

3. Expression interpolation:
```javascript
const a = 5;
const b = 10;
console.log(`Sum: ${a + b}, Product: ${a * b}`);
// Output: Sum: 15, Product: 50
```

4. Tagged templates:
```javascript
function highlight(strings, ...values) {
  return strings.reduce((acc, str, i) => 
    `${acc}${str}<strong>${values[i] || ''}</strong>`, '');
}

const name = "John";
const age = 30;
const result = highlight`My name is ${name} and I'm ${age} years old.`;
console.log(result);
// Output: My name is <strong>John</strong> and I'm <strong>30</strong> years old.
```

### 7. What is the difference between `==` and `===`?

The `==` (loose equality) and `===` (strict equality) operators are used for comparison in JavaScript, but they behave differently when comparing values of different types.

**Key differences:**
- `==` performs type coercion before comparison
- `===` does not perform type coercion and requires both value and type to be the same

**Examples:**

```javascript
// Loose equality (==)
console.log(5 == "5");     // true (string "5" is coerced to number 5)
console.log(0 == false);   // true (false is coerced to number 0)
console.log(null == undefined); // true (special case)

// Strict equality (===)
console.log(5 === "5");    // false (different types)
console.log(0 === false);  // false (different types)
console.log(null === undefined); // false (different types)

// More examples
console.log([] == 0);      // true ([] is coerced to 0)
console.log([] === 0);     // false (different types)
console.log('' == false);  // true (both coerced to 0)
console.log('' === false); // false (different types)
```

**Best practice:** It's generally recommended to use `===` for comparisons to avoid unexpected type coercion issues.

**Diagram: Equality Comparison**
```
┌─────────────────────┐    ┌─────────────────────┐
│ Loose Equality (==) │    │ Strict Equality (===)│
├─────────────────────┤    ├─────────────────────┤
│ 1. Type Coercion    │    │ 1. Type Check       │
│ 2. Value Comparison │    │ 2. Value Comparison │
└─────────────────────┘    └─────────────────────┘
```

### 8. What are the different ways to create an object in JavaScript?

There are several ways to create objects in JavaScript:

1. **Object literal notation:**
   ```javascript
   const obj = {
     name: "John",
     age: 30
   };
   ```

2. **Constructor function:**
   ```javascript
   function Person(name, age) {
     this.name = name;
     this.age = age;
   }
   const john = new Person("John", 30);
   ```

3. **Object.create() method:**
   ```javascript
   const personProto = {
     greet: function() {
       console.log(`Hello, I'm ${this.name}`);
     }
   };
   const john = Object.create(personProto);
   john.name = "John";
   ```

4. **ES6 Class syntax:**
   ```javascript
   class Person {
     constructor(name, age) {
       this.name = name;
       this.age = age;
     }
     greet() {
       console.log(`Hello, I'm ${this.name}`);
     }
   }
   const john = new Person("John", 30);
   ```

5. **Factory function:**
   ```javascript
   function createPerson(name, age) {
     return {
       name,
       age,
       greet() {
         console.log(`Hello, I'm ${this.name}`);
       }
     };
   }
   const john = createPerson("John", 30);
   ```

Each method has its own use cases and advantages. Object literals are great for simple objects, constructor functions and classes are useful for creating multiple instances with shared methods, `Object.create()` is powerful for setting up inheritance chains, and factory functions offer encapsulation and private variables.

### 9. What is the purpose of the `bind`, `call`, and `apply` methods?

`bind`, `call`, and `apply` are methods available on function objects in JavaScript. They are used to manipulate the `this` context of a function and, in the case of `call` and `apply`, to invoke the function immediately.

1. **`bind` method:**
   - Creates a new function with a fixed `this` context
   - Does not immediately invoke the function
   - Can partially apply arguments

   ```javascript
   const person = {
     name: "John",
     greet: function(greeting) {
       console.log(`${greeting}, I'm ${this.name}`);
     }
   };

   const greetJohn = person.greet.bind(person,"Hello");
   greetJohn(); // Output: Hello, I'm John
   ```

2. **`call` method:**
   - Invokes the function immediately with a specified `this` context
   - Accepts arguments individually

   ```javascript
   function greet(greeting, punctuation) {
     console.log(`${greeting}, I'm ${this.name}${punctuation}`);
   }

   const person = { name: "John" };
   greet.call(person, "Hello", "!"); // Output: Hello, I'm John!
   ```

3. **`apply` method:**
   - Similar to `call`, but accepts arguments as an array
   - Invokes the function immediately with a specified `this` context

   ```javascript
   function greet(greeting, punctuation) {
     console.log(`${greeting}, I'm ${this.name}${punctuation}`);
   }

   const person = { name: "John" };
   greet.apply(person, ["Hello", "!"]); // Output: Hello, I'm John!
   ```

**Key differences:**
- `bind` creates a new function, while `call` and `apply` invoke the function immediately
- `call` takes arguments individually, while `apply` takes arguments as an array

**Use cases:**
- `bind`: Creating methods with a fixed `this` context, partial function application
- `call`: Invoking methods in a specific context, borrowing methods from other objects
- `apply`: Similar to `call`, but useful when arguments are already in an array or when using `Math.max()/min()` with arrays

**Diagram: bind, call, and apply**
```
┌─────────────────────────────────────────────────────────────┐
│                    Function Manipulation                    │
├─────────────┬─────────────────────────┬─────────────────────┤
│    bind     │         call            │        apply        │
├─────────────┼─────────────────────────┼─────────────────────┤
│ New function│   Immediate invocation  │ Immediate invocation│
│ Fixed `this`│   Specified `this`      │   Specified `this`  │
│             │   Individual arguments  │   Array of arguments│
└─────────────┴─────────────────────────┴─────────────────────┘
```

### 10. What is event delegation?

Event delegation is a technique in JavaScript where you attach a single event listener to a parent element instead of attaching multiple listeners to individual child elements. This approach leverages event bubbling and can improve performance and simplify dynamic content handling.

**Key benefits:**
1. Improved performance (fewer event listeners)
2. Dynamically added elements are automatically handled
3. Less memory usage
4. Cleaner and more maintainable code

**Example:**
```html
<ul id="todo-list">
  <li>Task 1</li>
  <li>Task 2</li>
  <li>Task 3</li>
</ul>

<script>
document.getElementById('todo-list').addEventListener('click', function(e) {
  if (e.target.tagName === 'LI') {
    console.log('Clicked on:', e.target.textContent);
    e.target.style.textDecoration = 'line-through';
  }
});
</script>
```

In this example, instead of attaching click events to each `<li>` element, we attach a single listener to the parent `<ul>`. When a click occurs, we check if the clicked element (e.target) is an `<li>`, and if so, we handle the event.

**Diagram: Event Delegation**
```
┌─────────────────────────────┐
│       Parent Element        │
│  ┌───────────────────────┐  │
│  │   Event Listener      │  │
│  └───────────┬───────────┘  │
│    ▲         │         ▲    │
│    │         ▼         │    │
│ ┌──────┐ ┌──────┐ ┌──────┐ │
│ │Child │ │Child │ │Child │ │
│ └──────┘ └──────┘ └──────┘ │
└─────────────────────────────┘
        Event Bubbling
```