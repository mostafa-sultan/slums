# JavaScript Closures

## Introduction

Closures are one of the most important and powerful concepts in JavaScript. A closure gives you access to an outer function's scope from an inner function. Understanding closures is essential for writing effective JavaScript code.

---

## What is a Closure?

A closure is a function that has access to variables in its outer (enclosing) lexical scope, even after the outer function has returned. Closures are created every time a function is created.

### Basic Example

    function outerFunction() {
      const outerVariable = 'I am outside!';
      
      function innerFunction() {
        console.log(outerVariable); // Can access outerVariable
      }
      
      return innerFunction;
    }
    
    const myFunction = outerFunction();
    myFunction(); // Output: "I am outside!"

In this example, `innerFunction` forms a closure over `outerVariable`, allowing it to access the variable even after `outerFunction` has finished executing.

---

## How Closures Work

### Lexical Scoping

JavaScript uses lexical scoping, which means that the scope of a variable is determined by its location in the source code. Inner functions have access to variables in their outer scope.

    function outer() {
      let count = 0;
      
      function inner() {
        count++;
        console.log(count);
      }
      
      return inner;
    }
    
    const counter = outer();
    counter(); // 1
    counter(); // 2
    counter(); // 3

Each call to `counter()` increments the `count` variable, which persists because of the closure.

---

## Common Use Cases

### 1. Data Privacy and Encapsulation

Closures can be used to create private variables:

    function createCounter() {
      let count = 0; // Private variable
      
      return {
        increment: function() {
          count++;
          return count;
        },
        decrement: function() {
          count--;
          return count;
        },
        getCount: function() {
          return count;
        }
      };
    }
    
    const counter = createCounter();
    console.log(counter.increment()); // 1
    console.log(counter.increment()); // 2
    console.log(counter.getCount());   // 2
    // count is not directly accessible from outside

### 2. Function Factories

Closures enable function factories - functions that create and return other functions:

    function createMultiplier(multiplier) {
      return function(number) {
        return number * multiplier;
      };
    }
    
    const double = createMultiplier(2);
    const triple = createMultiplier(3);
    
    console.log(double(5));  // 10
    console.log(triple(5));  // 15

### 3. Event Handlers and Callbacks

Closures are commonly used in event handlers:

    function setupButton(buttonId, message) {
      const button = document.getElementById(buttonId);
      
      button.addEventListener('click', function() {
        alert(message); // Closure over 'message'
      });
    }
    
    setupButton('btn1', 'Hello from Button 1');
    setupButton('btn2', 'Hello from Button 2');

### 4. Module Pattern

Closures enable the module pattern for organizing code:

    const Calculator = (function() {
      let result = 0; // Private variable
      
      return {
        add: function(x) {
          result += x;
          return this;
        },
        subtract: function(x) {
          result -= x;
          return this;
        },
        multiply: function(x) {
          result *= x;
          return this;
        },
        getResult: function() {
          return result;
        },
        reset: function() {
          result = 0;
          return this;
        }
      };
    })();
    
    Calculator.add(10).multiply(2).subtract(5);
    console.log(Calculator.getResult()); // 15

---

## Closures in Loops

A common pitfall with closures occurs in loops:

### The Problem

    for (var i = 0; i < 3; i++) {
      setTimeout(function() {
        console.log(i); // Output: 3, 3, 3
      }, 1000);
    }

All functions share the same `i` variable, which is 3 by the time they execute.

### Solutions

**Solution 1: Use let instead of var**

    for (let i = 0; i < 3; i++) {
      setTimeout(function() {
        console.log(i); // Output: 0, 1, 2
      }, 1000);
    }

**Solution 2: IIFE (Immediately Invoked Function Expression)**

    for (var i = 0; i < 3; i++) {
      (function(j) {
        setTimeout(function() {
          console.log(j); // Output: 0, 1, 2
        }, 1000);
      })(i);
    }

**Solution 3: bind()**

    for (var i = 0; i < 3; i++) {
      setTimeout(function(j) {
        console.log(j); // Output: 0, 1, 2
      }.bind(null, i), 1000);
    }

---

## Advanced Closure Patterns

### Memoization

Closures can be used for memoization (caching function results):

    function memoize(fn) {
      const cache = {};
      
      return function(...args) {
        const key = JSON.stringify(args);
        
        if (cache[key]) {
          return cache[key];
        }
        
        const result = fn.apply(this, args);
        cache[key] = result;
        return result;
      };
    }
    
    const expensiveFunction = memoize(function(n) {
      console.log('Computing...');
      return n * n;
    });
    
    console.log(expensiveFunction(5)); // Computing... 25
    console.log(expensiveFunction(5)); // 25 (from cache)

### Partial Application

    function partial(fn, ...partialArgs) {
      return function(...remainingArgs) {
        return fn(...partialArgs, ...remainingArgs);
      };
    }
    
    function add(a, b, c) {
      return a + b + c;
    }
    
    const add5 = partial(add, 5);
    console.log(add5(10, 15)); // 30

### Currying

    function curry(fn) {
      return function curried(...args) {
        if (args.length >= fn.length) {
          return fn.apply(this, args);
        } else {
          return function(...nextArgs) {
            return curried.apply(this, args.concat(nextArgs));
          };
        }
      };
    }
    
    const add = curry((a, b, c) => a + b + c);
    console.log(add(1)(2)(3));    // 6
    console.log(add(1, 2)(3));   // 6
    console.log(add(1, 2, 3));    // 6

---

## Performance Considerations

### Memory Management

Closures keep references to outer variables, which can prevent garbage collection:

    function createLargeObject() {
      const largeData = new Array(1000000).fill('data');
      
      return function() {
        // largeData is kept in memory even if not used
        return 'Hello';
      };
    }

**Solution:** Set variables to null when done:

    function createLargeObject() {
      const largeData = new Array(1000000).fill('data');
      
      return function() {
        const result = 'Hello';
        largeData = null; // Help garbage collection
        return result;
      };
    }

---

## Best Practices

1. **Understand scope**: Know which variables are captured in closures
2. **Avoid memory leaks**: Don't keep unnecessary references
3. **Use closures for privacy**: Create private variables and methods
4. **Be careful in loops**: Use `let` or IIFE to avoid closure issues
5. **Document closures**: Make it clear when closures are used intentionally
6. **Test thoroughly**: Closures can have subtle bugs

---

## Common Mistakes

### Mistake 1: Accidental Closures

    function createFunctions() {
      const functions = [];
      for (var i = 0; i < 3; i++) {
        functions.push(function() {
          return i; // All return 3
        });
      }
      return functions;
    }

### Mistake 2: Memory Leaks

    function attachHandler() {
      const element = document.getElementById('button');
      const data = new Array(1000000).fill('data');
      
      element.onclick = function() {
        // data is kept in memory unnecessarily
        console.log('clicked');
      };
    }

---

## Conclusion

Closures are a fundamental concept in JavaScript that enable powerful programming patterns. They provide data privacy, enable function factories, and are essential for many advanced JavaScript patterns. Understanding closures will make you a better JavaScript developer and help you write more efficient and maintainable code.

