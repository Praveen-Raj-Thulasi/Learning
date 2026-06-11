# React Basics & Introduction

![React Logo](https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg)

---

# Introduction to React

## What is React?

React is a popular JavaScript library used for building fast and interactive user interfaces, especially for single-page applications (SPAs).

It was developed by Meta (formerly Facebook) and is widely used for modern frontend web development.

React helps developers create reusable UI components and efficiently update the webpage whenever data changes.

---

# Why Use React?

## Advantages of React

- Component-based architecture
- Reusable code
- Fast rendering using Virtual DOM
- Easy state management
- Large ecosystem and community support

---

# React vs Traditional JavaScript

| Traditional JS | React |
|---|---|
| Manual DOM updates | Automatic UI updates |
| Hard to manage large apps | Easier component management |
| Less reusable | Highly reusable |
| More boilerplate | Cleaner structure |

---

# What is a Component?

A component is a reusable piece of UI.

Example:
- Navbar
- Button
- Footer
- Product Card

---

## Component Illustration

![React Components](https://miro.medium.com/v2/resize:fit:1400/1*8OlyP02fUPjXrkxJxN4M0w.png)

---

# Types of Components

## 1. Functional Components

```jsx
function Welcome() {
  return <h1>Hello React</h1>;
}
```

---

## 2. Class Components

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello React</h1>;
  }
}
```

---

# JSX (JavaScript XML)

JSX allows writing HTML inside JavaScript.

Example:

```jsx
const element = <h1>Hello World</h1>;
```

---

# Virtual DOM

React uses a Virtual DOM to improve performance.

## How it works:

1. React creates a virtual copy of the real DOM.
2. Changes are made in the Virtual DOM.
3. React compares old and new Virtual DOM.
4. Only changed elements are updated in the browser.

---

## Virtual DOM Diagram

![Virtual DOM](https://miro.medium.com/v2/resize:fit:1400/1*ZwMog6Vb1XcR2gqJb7t0xQ.png)

---

# Installing React

## Using Vite (Recommended)

```bash
npm create vite@latest my-app
cd my-app
npm install
npm run dev
```

---

# React Project Structure

```text
my-app/
│
├── public/
├── src/
│   ├── components/
│   ├── App.jsx
│   ├── main.jsx
│
├── package.json
```

---

# React Props

Props are used to pass data between components.

```jsx
function User(props) {
  return <h1>{props.name}</h1>;
}

<User name="Praveen" />
```

---

# React State

State stores dynamic data inside components.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      {count}
    </button>
  );
}
```

---

# React Hooks

Hooks allow functional components to use React features.

## Common Hooks

| Hook | Purpose |
|---|---|
| useState | Manage state |
| useEffect | Handle side effects |
| useRef | Access DOM elements |
| useContext | Global state sharing |

---

# useEffect Example

```jsx
import { useEffect } from "react";

useEffect(() => {
  console.log("Component Mounted");
}, []);
```

---

# Event Handling

```jsx
function Button() {
  function handleClick() {
    alert("Button Clicked");
  }

  return <button onClick={handleClick}>Click</button>;
}
```

---

# Conditional Rendering

```jsx
function App() {
  const isLoggedIn = true;

  return (
    <>
      {isLoggedIn ? <h1>Welcome</h1> : <h1>Please Login</h1>}
    </>
  );
}
```

---

# Lists in React

```jsx
const items = ["Apple", "Banana", "Mango"];

function App() {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}
```

---

# React Lifecycle (Simplified)

## Phases

1. Mounting
2. Updating
3. Unmounting

---

# Popular React Libraries

| Library | Purpose |
|---|---|
| React Router | Routing |
| Redux | State Management |
| Axios | API Calls |
| Tailwind CSS | Styling |
| Material UI | UI Components |

---

# Fetching API Data

```jsx
useEffect(() => {
  fetch("https://api.example.com/data")
    .then(res => res.json())
    .then(data => console.log(data));
}, []);
```

---

# Advantages of React

- Fast
- Scalable
- Reusable Components
- SEO Friendly (with Next.js)
- Huge community support

---

# Limitations of React

- Only handles UI
- Fast-changing ecosystem
- Requires additional libraries

---

# Simple React App Example

```jsx
function App() {
  return (
    <div>
      <h1>Hello React</h1>
      <p>Welcome to React Basics</p>
    </div>
  );
}

export default App;
```

---

# Conclusion

React is one of the most powerful frontend technologies for building modern web applications.

By learning:
- Components
- JSX
- Props
- State
- Hooks

you can start building interactive and scalable applications efficiently.

---

# Recommended Learning Path

1. JavaScript Fundamentals
2. ES6 Features
3. React Basics
4. Hooks
5. Routing
6. API Integration
7. State Management
8. Full Stack Projects

---

# Official Resources

- https://react.dev/
- https://vitejs.dev/
- https://reactrouter.com/
