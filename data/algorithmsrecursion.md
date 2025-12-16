# Recursion in Algorithms

## Introduction

Recursion is a technique where a function calls itself. This tutorial covers recursive algorithms, base cases, and optimization.

---

## Factorial

    function factorial(n) {
      if (n <= 1) return 1; // Base case
      return n * factorial(n - 1); // Recursive case
    }

---

## Fibonacci

    function fibonacci(n) {
      if (n <= 1) return n;
      return fibonacci(n - 1) + fibonacci(n - 2);
    }

---

## Conclusion

Use recursion for problems that can be broken down into smaller subproblems. Always define base cases to prevent infinite recursion.

