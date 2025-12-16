# Dynamic Programming

## Introduction

Dynamic programming solves complex problems by breaking them into simpler subproblems. This tutorial covers memoization and tabulation.

---

## Memoization

    function fibonacciMemo(n, memo = {}) {
      if (n in memo) return memo[n];
      if (n <= 1) return n;
      memo[n] = fibonacciMemo(n - 1, memo) + fibonacciMemo(n - 2, memo);
      return memo[n];
    }

---

## Conclusion

Use dynamic programming to optimize recursive solutions. Memoization stores results to avoid redundant calculations.

