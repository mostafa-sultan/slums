# State Management in React

## Introduction

State management is crucial for React applications. This tutorial covers useState, useReducer, Context API, and Redux.

---

## Local State

    function Counter() {
      const [count, setCount] = useState(0);
      return <button onClick={() => setCount(count + 1)}>{count}</button>;
    }

---

## Context API

    const CountContext = createContext();
    
    function App() {
      const [count, setCount] = useState(0);
      return (
        <CountContext.Provider value={{ count, setCount }}>
          <Component />
        </CountContext.Provider>
      );
    }

---

## Redux

    import { createStore } from 'redux';
    
    function reducer(state = { count: 0 }, action) {
      switch (action.type) {
        case 'INCREMENT': return { count: state.count + 1 };
        default: return state;
      }
    }
    
    const store = createStore(reducer);

---

## Conclusion

Choose the right state management solution based on your app's complexity. Start with local state, use Context for shared state, and Redux for complex applications.

