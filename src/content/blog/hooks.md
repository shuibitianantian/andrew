---
author: Andrew Xia
pubDatetime: 2024-02-20T17:21:32.400Z
modDatetime: 2024-02-20T17:21:32.400Z
title: React hooks
slug:  React hooks
draft: false
tags:
  - docs
description: Simple introduction about react hooks
---

# Hooks

### useState

**`useState`** is a hook that lets you add React state to function components. It’s a function that returns an array with two elements:

1. The current state value.
2. A function that allows you to update that state.

```tsx
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable called "count"
  const [count, setCount] = useState(0); // init

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

**Explanation**

- **Initialization**: **`useState(0)`** initializes the state variable **`count`** with a value of **`0`**. **`useState`** can be used with any type of value you need, such as numbers, strings, arrays, objects, etc.
- **Array Destructuring**: The **`useState`** function returns an array with two elements, and we use array destructuring to access them. The first element is the current value of the state, and the second element is a function that allows updating that value.
- **Updating State**: Call the **`setCount`** function with a new value to update **`count`** and re-render the component with the updated state.

**Key Points to Emphasize**

- **Component Isolation**: Each call to **`useState`** creates an isolated state for the component. Multiple calls support multiple state variables. **Tricky!!!!!**
- **Functional Updates**: If the new state is computed using the previous state, you can pass a function to the setter function. For example, **`setCount(prevCount => prevCount + 1)`**.
- **Re-rendering**: Updating the state via the setter function triggers a re-render of the component, ensuring the UI reflects the current state.

### useEffect

The **`useEffect`** hook is a function provided by React that allows you to perform side effects in function components. Side effects are operations that can affect other components and cannot be done during rendering. Common examples of side effects include fetching data, directly updating the DOM, and timers.

```tsx
useEffect(effectFunction, dependencies);
```

- **`effectFunction`**: A function where you can place your side effect logic. This function can optionally return a cleanup function that React will run to clean up the current effect before re-running it or when the component unmounts.
- **`dependencies`**: An optional array of dependencies that, if specified, determines when the effect should re-run. If the values in the array haven’t changed between re-renders, React will skip the effect. If omitted, the effect runs after every render. If you pass an empty array (**`[]`**), the effect runs once after the initial render, making it similar to **`componentDidMount`** in class components.

Examples:

```tsx
useEffect(() => {
  // This code runs if `count` changes
  document.title = `You clicked ${count} times`;
}, [count]); // This effect depends on `count`

useEffect(() => {
  // This code runs once after the initial render
  fetchData();
}, []); // The empty array means no dependencies, so the effect doesn't re-run
```

### **Cleaning Up an Effect**

Some effects require cleanup to prevent memory leaks, such as clearing timers or canceling network requests. To do this, your effect function can return a cleanup function:

```tsx
useEffect(() => {
  const timer = setTimeout(() => {
    alert("This runs after 1 second.");
  }, 1000);

  // Cleanup function
  return () => clearTimeout(timer);
}, []); // Dependencies array is empty, so this effect runs once
```

**Key Points to Emphasize**

- **Separation of Concerns**: Unlike lifecycle methods in class components, **`useEffect`** allows you to group code by what it does rather than when it runs. This leads to more readable and maintainable components.
- **Multiple Effects**: You can use multiple **`useEffect`** calls within a single component to separate unrelated logic.
- **Conditional Execution**: The dependency array allows you to control when your effects run, optimizing performance by preventing unnecessary work.
