# Testing React Applications

## Introduction

Testing ensures your React applications work correctly. This tutorial covers Jest, React Testing Library, and testing best practices.

---

## Jest Setup

    // sum.test.js
    test('adds 1 + 2 to equal 3', () => {
      expect(sum(1, 2)).toBe(3);
    });

---

## React Testing Library

    import { render, screen } from '@testing-library/react';
    import App from './App';
    
    test('renders learn react link', () => {
      render(<App />);
      const linkElement = screen.getByText(/learn react/i);
      expect(linkElement).toBeInTheDocument();
    });

---

## Testing Hooks

    import { renderHook, act } from '@testing-library/react';
    import { useCounter } from './useCounter';
    
    test('increments counter', () => {
      const { result } = renderHook(() => useCounter());
      act(() => {
        result.current.increment();
      });
      expect(result.current.count).toBe(1);
    });

---

## Conclusion

Write tests for your React components and hooks. Use React Testing Library for component testing and Jest for unit tests.

