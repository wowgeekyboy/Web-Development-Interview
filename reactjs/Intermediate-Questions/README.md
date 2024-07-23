# 🚀 React.js Interview Questions and Answers

## Intermediate Questions

### 1. What is the significance of the `key` prop in React? 🔑

We covered this briefly in the basic questions, but let's delve deeper into the significance of the `key` prop.

**Expert Explanation:** The `key` prop is a special attribute that helps React identify which items in a list have changed, been added, or been removed. It's crucial for optimizing the rendering of dynamic lists.

Key points:
1. **Unique Identification:** Keys should be unique among siblings, but they don't need to be globally unique.
2. **Reconciliation:** Keys help React's reconciliation process by allowing it to reuse existing DOM elements when possible.
3. **State Preservation:** Proper key usage ensures that component state is preserved even when the list order changes.
4. **Performance:** Using keys correctly can significantly improve the performance of list rendering.

**Example with Performance Implications:**
```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo, index) => (
        // Bad practice: using index as key
        <li key={index}>{todo.text}</li>
      ))}
    </ul>
  );
}

// Better practice: using unique id as key
function ImprovedTodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

In the first example, using the index as a key can lead to issues if the list order changes or items are added/removed from the middle of the list. The second example uses a unique `id`, which is more stable and leads to better performance and fewer unexpected behaviors.

### 2. How do you handle events in React? 🖱️

Handling events in React is similar to handling events in DOM, but with some syntactic differences.

**Novice Explanation:** Imagine you're setting up a doorbell 🚪🔔. When someone presses the button, you want something to happen. In React, we set up these "doorbells" (events) on our components and decide what should happen when they're triggered.

**Expert Explanation:** React events are named using camelCase and passed as functions rather than strings. They are synthetic events, wrapping the browser's native events for cross-browser compatibility.

Key points:
1. React uses synthetic events for better cross-browser compatibility.
2. Event handlers are passed as functions, not strings.
3. You can't return `false` to prevent default behavior; you must call `preventDefault` explicitly.
4. In class components, you need to bind `this` to event handlers or use arrow functions.

**Example:**
```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // Binding necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

// Functional component with hooks
function Toggle() {
  const [isToggleOn, setIsToggleOn] = useState(true);

  const handleClick = () => {
    setIsToggleOn(prevState => !prevState);
  };

  return (
    <button onClick={handleClick}>
      {isToggleOn ? 'ON' : 'OFF'}
    </button>
  );
}
```

### 3. What are hooks in React? Can you name a few? 🎣

Hooks are functions that let you "hook into" React state and lifecycle features from functional components.

**Novice Explanation:** Imagine fishing 🎣. Hooks in React are like fishing hooks - they let you catch and use special React features in your functional components, just like a fishing hook lets you catch fish in a lake.

**Expert Explanation:** Hooks were introduced in React 16.8 to allow developers to use state and other React features without writing a class. They solve several problems in React:

1. Reusing stateful logic between components
2. Splitting complex components into smaller functions
3. Using React features without classes

Common built-in hooks:
- `useState`: Adds state to functional components
- `useEffect`: Performs side effects in functional components
- `useContext`: Subscribes to React context
- `useReducer`: Manages complex state logic
- `useCallback`: Memoizes callbacks for optimized child components
- `useMemo`: Memoizes expensive computations
- `useRef`: Creates a mutable reference that persists across re-renders

**Example:**
```jsx
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### 4. What is the `useState` hook? 🔢

The `useState` hook allows functional components to manage state.

**Novice Explanation:** Think of `useState` like a magic box 📦. You can put something in it (your initial state), and it gives you back two things: the current thing in the box, and a magic wand to change what's in the box.

**Expert Explanation:** `useState` is a hook that lets you add React state to functional components. It returns an array with two elements: the current state value and a function to update it.

Key points:
1. The update function can take a new state value or a function that returns the new state.
2. Unlike `this.setState` in class components, `useState` does not automatically merge update objects.
3. You can use the State Hook more than once in a single component.
4. The initial state argument is only used during the first render.

**Example:**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### 5. What is the `useEffect` hook and how does it work? 🔄

The `useEffect` hook lets you perform side effects in functional components.

**Novice Explanation:** Imagine you have a robot helper 🤖. `useEffect` is like giving instructions to your robot: "Every time something changes, do this task." It helps your component react to changes and do things outside of just rendering.

**Expert Explanation:** `useEffect` serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React class lifecycle methods, but unified into a single API.

Key points:
1. It runs after every render, including the first render.
2. You can control when it runs by passing a dependency array.
3. It can return a cleanup function to remove event listeners, cancel subscriptions, etc.
4. Multiple effects can be used to separate concerns.

**Example:**
```jsx
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  }, [props.friend.id]); // Only re-run the effect if props.friend.id changes

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

### 6. What is context in React and how is it used? 🌳

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

**Novice Explanation:** Imagine a family tree 🌳. Instead of passing a message from grandparent to parent to child, Context lets the grandparent send a message directly to any descendant who's listening, skipping all the levels in between.

**Expert Explanation:** Context is designed to share data that can be considered "global" for a tree of React components, such as the current authenticated user, theme, or preferred language.

Key points:
1. Context helps avoid "prop drilling" (passing props through intermediate elements).
2. It's useful for sharing data that is considered global for a tree of React components.
3. Context uses a provider-consumer pattern.
4. The `useContext` hook can be used to consume context in functional components.

**Example:**
```jsx
// Create a context
const ThemeContext = React.createContext('light');

// Provider component
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

// Intermediate component
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

// Consumer component using useContext
function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <Button theme={theme} />;
}
```

### 7. How can you optimize performance in a React application? 🚀

Optimizing performance in React involves several strategies to minimize unnecessary renders and improve overall application speed.

**Novice Explanation:** Imagine you're organizing a race 🏁. To make the cars go faster, you might make them lighter, give them better fuel, or smooth out the track. Similarly, in React, we can make our app faster by making our components smarter about when they need to update, using better data structures, or avoiding unnecessary work.

**Expert Explanation:** Performance optimization in React can be achieved through various techniques:

1. **Use Production Build**: Always use the production build of React for deployment.

2. **Virtualization**: For long lists, use virtualization libraries like `react-window`.

3. **Memoization**: Use `React.memo` for functional components and `PureComponent` for class components to prevent unnecessary re-renders.

4. **Code Splitting**: Use dynamic `import()` and `React.lazy` for code splitting.

5. **Debouncing and Throttling**: Implement these techniques for performance-intensive operations.

6. **Avoid Inline Function Definition**: Define functions outside the render method to prevent unnecessary re-creation.

7. **Use keys properly**: Ensure proper use of keys in lists for efficient reconciliation.

8. **Avoid Unnecessary Renders**: Implement `shouldComponentUpdate` or use `React.memo` wisely.

**Example of Memoization:**
```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveComputationComponent({ computationParam }) {
  const expensiveResult = useMemo(() => {
    // Expensive computation here
    return computeExpensiveValue(computationParam);
  }, [computationParam]);

  return <div>{expensiveResult}</div>;
}

// Usage
function App() {
  const [count, setCount] = useState(0);
  const [computationParam, setComputationParam] = useState(10);

  return (
    <div>
      <ExpensiveComputationComponent computationParam={computationParam} />
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <button onClick={() => setComputationParam(computationParam + 1)}>Change Computation Param</button>
    </div>
  );
}
```

In this example, the expensive computation is only re-run when `computationParam` changes, not on every render of the parent component.

### 8. What is the purpose of `React.memo`? 🧠

`React.memo` is a higher-order component that can be used to optimize the performance of functional components by memoizing the result.

**Novice Explanation:** Imagine you have a friend who's really good at math 🧮. Instead of asking them to solve the same problem over and over, you write down their answer and reuse it when you get the same question. `React.memo` does something similar for your components.

**Expert Explanation:** `React.memo` is used to wrap functional components to prevent unnecessary re-renders. It works by doing a shallow comparison of the component's props. If the props haven't changed, React reuses the last rendered result, skipping the rendering process for the component and its children.

Key points:
1. It's similar to `PureComponent` for class components.
2. By default, it only shallowly compares complex objects in the props object.
3. You can provide a custom comparison function as the second argument.
4. It's most beneficial for components that render often with the same props.

**Example:**
```jsx
import React, { useState } from 'react';

const ExpensiveComponent = React.memo(({ value }) => {
  console.log('Rendering ExpensiveComponent');
  // Imagine some expensive computation here
  return <div>{value}</div>;
});

function App() {
  const [count, setCount] = useState(0);
  const [value, setValue] = useState('Hello');

  return (
    <div>
      <ExpensiveComponent value={value} />
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <button onClick={() => setValue('World')}>Change Value</button>
    </div>
  );
}
```

In this example, `ExpensiveComponent` will only re-render when the `value` prop changes, not when `count` is incremented.

### 9. What is a higher-order component (HOC)? 🏗️

A higher-order component is a function that takes a component and returns a new component with some additional functionality or props.

**Novice Explanation:** Think of a HOC like a special machine in a toy factory 🏭. You put in a toy, and the machine adds some cool new features to it before giving it back. HOCs do the same thing with components - they add new abilities to existing components.

**Expert Explanation:** HOCs are a powerful technique for reusing component logic. They are a pattern that emerges from React's compositional nature.

Key points:
1. HOCs are pure functions with no side effects.
2. They don't modify the input component; they compose the original component by wrapping it in a container component.
3. HOCs can be used to abstract common functionality that's used across multiple components.
4. They're commonly used for cross-cutting concerns like logging, access control, or data fetching.

**Example:**
```jsx
// Higher-order component
function withLogging(WrappedComponent) {
  return class extends React.Component {
    componentDidMount() {
      console.log(`Component ${WrappedComponent.name} mounted`);
    }
    
    render() {
      return <WrappedComponent {...this.props} />;
    }
  }
}

// Simple component
function Button({ label }) {
  return <button>{label}</button>;
}

// Enhanced component
const EnhancedButton = withLogging(Button);

// Usage
function App() {
  return <EnhancedButton label="Click me" />;
}
```

In this example, `withLogging` is a HOC that adds logging functionality to any component it wraps.

### 10. What are fragments in React? 🧩

Fragments let you group a list of children without adding extra nodes to the DOM.

**Novice Explanation:** Imagine you have a bunch of stickers 🏷️, but you're only allowed to stick them in one place. Fragments are like an invisible sheet that lets you group your stickers together so you can stick them all at once without seeing the sheet.

**Expert Explanation:** Fragments solve the common pattern of returning multiple elements from a component's render method without creating an unnecessary parent DOM node.

Key points:
1. Fragments don't create an extra DOM node, unlike a containing `<div>`.
2. They can be written with the explicit `<React.Fragment>` syntax or the shorthand `<>` syntax.
3. The explicit syntax allows you to specify a `key` when rendering lists of fragments.
4. Fragments can make the DOM tree flatter and cleaner.

**Example:**
```jsx
function Table() {
  return (
    <table>
      <tbody>
        <tr>
          <Columns />
        </tr>
      </tbody>
    </table>
  );
}

function Columns() {
  return (
    <>
      <td>Hello</td>
      <td>World</td>
    </>
  );
}
```

In this example, using a Fragment in the `Columns` component allows us to group the `<td>` elements without adding an extra `<div>` that would make the HTML invalid.

### 11. What is the purpose of the `useContext` hook? 🌍

The `useContext` hook allows functional components to consume context without introducing nesting.

**Novice Explanation:** Imagine you're in a big building 🏢 with a speaker system. `useContext` is like having a special earpiece that lets you hear announcements meant for everyone in the building, no matter which room you're in.

**Expert Explanation:** `useContext` provides a way to pass data through the component tree without having to pass props down manually at every level. It's particularly useful for sharing data that can be considered "global" for a tree of React components.

Key points:
1. It subscribes to context changes and updates when the context value changes.
2. It takes a context object (the value returned from `React.createContext`) as its argument.
3. It returns the current context value for that context.
4. The component using `useContext` will always re-render when the context value changes.

**Example:**
```jsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>I am styled by theme context!</button>;
}

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <ThemedButton />
    </ThemeContext.Provider>
  );
}
```

In this example, `ThemedButton` uses the `useContext` hook to access the current theme without needing to use a Context.Consumer or receive it via props.

### 12. What are custom hooks? 🎣

Custom hooks are JavaScript functions that start with "use" and may call other hooks. They allow you to extract component logic into reusable functions.

**Novice Explanation:** Custom hooks are like creating your own Swiss Army knife 🔪. You take the tools (built-in hooks) that React gives you and combine them in a way that's useful for your specific needs.

**Expert Explanation:** Custom hooks enable you to reuse stateful logic between components without changing their hierarchy. They follow the same rules as the built-in hooks:

1. Only call hooks at the top level of your React function.
2. Only call hooks from React function components or custom hooks.

Key benefits:
1. Reusability: Share logic between components.
2. Abstraction: Hide complex logic behind a simple interface.
3. Composition: Combine multiple hooks into a single custom hook.
4. Testability: Custom hooks can be easily unit tested.

**Example:**
```jsx
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);
  
  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return width;
}

// Usage in a component
function ResponsiveComponent() {
  const width = useWindowWidth();
  return <div>Window width is {width}</div>;
}
```

In this example, `useWindowWidth` is a custom hook that manages its own state and side effects. It can be reused across multiple components that need to know the current window width.

