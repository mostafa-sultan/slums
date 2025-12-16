# React Hooks Complete Guide

## Introduction

React Hooks allow you to use state and other React features in functional components. This tutorial covers all built-in hooks and custom hooks.

---

## useState

    import { useState } from 'react';
    
    function Counter() {
      const [count, setCount] = useState(0);
      
      return (
        <div>
          <p>Count: {count}</p>
          <button onClick={() => setCount(count + 1)}>Increment</button>
        </div>
      );
    }

---

## useEffect

    import { useState, useEffect } from 'react';
    
    function UserProfile({ userId }) {
      const [user, setUser] = useState(null);
      
      useEffect(() => {
        fetchUser(userId).then(setUser);
      }, [userId]);
      
      return <div>{user?.name}</div>;
    }

---

## useContext

    const ThemeContext = createContext();
    
    function App() {
      return (
        <ThemeContext.Provider value="dark">
          <Component />
        </ThemeContext.Provider>
      );
    }
    
    function Component() {
      const theme = useContext(ThemeContext);
      return <div>Theme: {theme}</div>;
    }

---

## useReducer

    function reducer(state, action) {
      switch (action.type) {
        case 'increment': return { count: state.count + 1 };
        case 'decrement': return { count: state.count - 1 };
        default: return state;
      }
    }
    
    function Counter() {
      const [state, dispatch] = useReducer(reducer, { count: 0 });
      return (
        <div>
          <p>Count: {state.count}</p>
          <button onClick={() => dispatch({ type: 'increment' })}>+</button>
        </div>
      );
    }

---

## Custom Hooks

    function useCounter(initialValue = 0) {
      const [count, setCount] = useState(initialValue);
      const increment = () => setCount(c => c + 1);
      const decrement = () => setCount(c => c - 1);
      return { count, increment, decrement };
    }

---

## Conclusion

Hooks make React code more reusable and easier to understand. Master these hooks to build better React applications.

