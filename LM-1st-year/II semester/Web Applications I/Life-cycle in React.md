# Hook: `useEffect`

In React, a "side effect" refers to any operation or interaction that affects something outside the scope of the current function being executed. This includes operations such as data fetching, setting up subscriptions or event listeners, logging, and manipulating the DOM manually.

### What is a Side Effect?

A side effect is any action that interacts with the outside world or has an observable effect beyond returning a value. For instance:
- **Data fetching**: Requesting data from an API.
- **Logging**: Recording logs or metrics.
- **Subscriptions**: Setting up or tearing down subscriptions to external data sources.
- **Timers**: Setting up intervals or timeouts.
- **Manual DOM updates**: Directly manipulating the DOM elements.

### Why Avoid Side Effects in Rendering

React components are primarily functions that take props and state to compute and return JSX. Side effects should not be performed directly within the component's main body (i.e., during rendering), because rendering should remain a pure operation. Pure functions with no side effects are predictable and easier to debug. React separates the rendering phase from side effects to maintain these properties.

### How to Use `useEffect`

The `useEffect` hook is the mechanism to perform side effects in functional React components. It allows you to execute side effects after the component has rendered.

#### Syntax

```javascript
useEffect(callback, [dependencies])
```

- **`callback`**: A function containing the side effect logic.
- **`dependencies`**: An optional array of dependencies that determine when the effect should re-run.

#### Example

```javascript
import React, { useState, useEffect } from 'react';

function ExampleComponent() {
  const [data, setData] = useState(null);
  const [count, setCount] = useState(0);

  useEffect(() => {
    // This is the side effect: fetching data from an API
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []); // Empty array means this effect runs only once after the initial render

  useEffect(() => {
    // This effect runs every time `count` changes
    console.log(`Count has changed to: ${count}`);
  }, [count]); // Dependency array with `count` means this effect runs when `count` changes

  return (
    <div>
      <p>Data: {data}</p>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### Key Concepts of `useEffect`

1. **Dependencies**: The effect runs only when any value in the dependency array changes. If the array is empty (`[]`), the effect runs only once after the initial render. If no array is provided, the effect runs after every render.
2. **Cleanup Function**: The `callback` function can optionally return a cleanup function to clean up resources (e.g., removing event listeners, canceling network requests) when the component unmounts or before the effect runs again.

#### Cleanup Example

```javascript
useEffect(() => {
  const handleResize = () => {
    console.log('Window resized');
  };

  window.addEventListener('resize', handleResize);

  return () => {
    // Cleanup function to remove the event listener
    window.removeEventListener('resize', handleResize);
  };
}, []); // Empty array to ensure this effect runs only once
```

### Common Mistakes

1. **Missing Dependencies**: If you omit necessary dependencies, your effect might use stale values from previous renders.
2. **Infinite Loops**: If your effect updates a dependency that it depends on without careful handling, it could cause an infinite loop of rendering and running the effect.

#### Avoiding Infinite Loops Example

```javascript
useEffect(() => {
  setCount(prevCount => prevCount + 1);
}, [count]); // Incorrect: This will cause an infinite loop

// Correct approach:
useEffect(() => {
  setCount(prevCount => prevCount + 1);
}, []); // This runs only once
```

By using `useEffect` correctly, you can effectively manage side effects in your React components, ensuring that your application behaves as expected and remains maintainable.