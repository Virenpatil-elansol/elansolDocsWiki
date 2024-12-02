# 📚 **React Crash Course Documentation**  

Welcome to the **React Crash Course**! This guide will walk you through the basics of **React**, focusing on hooks and real-life use cases. Whether you're a beginner or looking to brush up on the latest React practices, this guide has you covered. We'll explore installation, dive into essential hooks, and provide practical examples to help you apply React concepts in real-world scenarios.  

---

## 🚀 **Getting Started with React**  

To begin working with React, you'll need to install **Node.js** and set up a React project using either **Create React App** or **Vite**.  

### **Step 1: Install Node.js**  
Download and install **Node.js** from [nodejs.org](https://nodejs.org/). It includes npm (Node Package Manager) to help you manage dependencies.  

### **Step 2: Set Up a React Project**  
The easiest way to set up a React project is with **Create React App**. Run the following commands to get started:  

```bash
npx create-react-app my-react-app --template typescript
cd my-react-app
npm start
```  

This will create a new React project with TypeScript support and launch it in your default browser.  

---

## 🧑‍💻 **Basic Concepts of React**  

Before we dive into hooks, here’s a quick overview of React's foundational concepts:  

1. **Components**: React apps are built with components—JavaScript functions that return JSX to define what should be rendered on the screen.  
2. **JSX**: A syntax extension for JavaScript that resembles HTML but supports embedding JavaScript expressions.  
3. **Props**: Used to pass data from parent to child components.  
4. **State**: A local data store within a component that changes dynamically.  

---

## 🔨 **React Hooks**  

React hooks simplify the process of managing state, side effects, and lifecycle methods in functional components. Here are the most commonly used hooks:  

---

### **1. useState**  

`useState` allows you to add state to your functional components.  

#### **How to Use `useState`**  

```tsx
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};

export default Counter;
```  

#### **Real-Life Example:**  
A **counter application** where users can increment or decrement a value.  

---

### **2. useEffect**  

`useEffect` handles side effects like data fetching, DOM updates, or setting up subscriptions.  

#### **How to Use `useEffect`**  

```tsx
import React, { useState, useEffect } from 'react';

const FetchDataComponent = () => {
  const [data, setData] = useState<string | null>(null);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts/1')
      .then((response) => response.json())
      .then((jsonData) => setData(jsonData.title));
  }, []); // Runs once on mount

  return <div>{data ? data : 'Loading...'}</div>;
};

export default FetchDataComponent;
```  

#### **Real-Life Example:**  
Fetching and displaying a **post title** from a placeholder API.  

---

### **3. useContext**  

`useContext` provides a way to share data (like themes or authentication) across components without passing props manually.  

#### **How to Use `useContext`**  

```tsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

const ThemedComponent = () => {
  const theme = useContext(ThemeContext);

  return (
    <div style={{ background: theme === 'dark' ? '#333' : '#fff', color: theme === 'dark' ? '#fff' : '#000' }}>
      The current theme is {theme}
    </div>
  );
};

const App = () => (
  <ThemeContext.Provider value="dark">
    <ThemedComponent />
  </ThemeContext.Provider>
);

export default App;
```  

#### **Real-Life Example:**  
A **theme toggle** system that dynamically updates the UI based on the selected theme.  

---

### **4. useReducer**  

`useReducer` is ideal for managing complex state logic.  

#### **How to Use `useReducer`**  

```tsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state: { count: number }, action: { type: string }) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: return state;
  }
}

const CounterWithReducer = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <h1>Count: {state.count}</h1>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
    </div>
  );
};

export default CounterWithReducer;
```  

#### **Real-Life Example:**  
A **complex counter application** using `useReducer` for better state control.  

---

### **5. useRef**  

`useRef` persists values across renders without triggering re-renders.  

#### **How to Use `useRef`**  

```tsx
import React, { useRef } from 'react';

const FocusInputComponent = () => {
  const inputRef = useRef<HTMLInputElement>(null);

  const handleFocus = () => {
    if (inputRef.current) inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
};

export default FocusInputComponent;
```  

#### **Real-Life Example:**  
A **focus input** feature that programmatically focuses an input element.  

---

### **6. useMemo**  

`useMemo` optimizes expensive calculations by memoizing their results.  

#### **How to Use `useMemo`**  

```tsx
import React, { useState, useMemo } from 'react';

const ExpensiveComputation = ({ num }: { num: number }) => {
  const result = useMemo(() => {
    console.log('Calculating...');
    return num * 1000;
  }, [num]);

  return <div>Result: {result}</div>;
};

const MemoExample = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <ExpensiveComputation num={count} />
    </div>
  );
};

export default MemoExample;
```  

#### **Real-Life Example:**  
Avoiding unnecessary recalculations in an **expensive computation component**.  

---

### **7. useCallback**  

`useCallback` memoizes functions to prevent unnecessary re-renders.  

#### **How to Use `useCallback`**  

```tsx
import React, { useState, useCallback } from 'react';

const Button = ({ onClick }: { onClick: () => void }) => {
  console.log('Rendering Button');
  return <button onClick={onClick}>Click Me</button>;
};

const CallbackExample = () => {
  const [count, setCount] = useState(0);
  const increment = useCallback(() => setCount(count + 1), [count]);

  return (
    <div>
      <Button onClick={increment} />
      <h1>Count: {count}</h1>
    </div>
  );
};

export default CallbackExample;
```  

#### **Real-Life Example:**  
A **memoized click handler** for buttons to optimize rendering.  

---

## 📁 **Folder Structure**  

Here’s an example folder structure for a scalable React app:  

```plaintext
/my-react-app
  /src
    /components     # Reusable components
    /hooks          # Custom hooks
    /pages          # Page components for routing
    App.tsx         # Main entry point
  /public
    index.html
  tsconfig.json     # TypeScript configuration
  package.json      # Project dependencies
```  

---

## 🚀 **Conclusion**  

This crash course equips you with the knowledge to start building functional React applications using hooks. Experiment with these concepts, integrate them into your projects, and refine your development workflow.  

**Happy coding!**  
