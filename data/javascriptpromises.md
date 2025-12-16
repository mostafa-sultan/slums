# JavaScript Promises and Async Programming

## Introduction

Promises are a powerful feature in JavaScript for handling asynchronous operations. They provide a cleaner alternative to callbacks and make it easier to handle asynchronous code. This tutorial covers everything you need to know about Promises, async/await, and asynchronous programming in JavaScript.

---

## What are Promises?

A Promise is an object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. A Promise can be in one of three states:

- **Pending**: Initial state, neither fulfilled nor rejected
- **Fulfilled**: Operation completed successfully
- **Rejected**: Operation failed

### Creating a Promise

    const promise = new Promise((resolve, reject) => {
      // Asynchronous operation
      setTimeout(() => {
        const success = true;
        if (success) {
          resolve('Operation successful!');
        } else {
          reject('Operation failed!');
        }
      }, 1000);
    });

---

## Basic Promise Usage

### then() and catch()

    promise
      .then(result => {
        console.log(result); // "Operation successful!"
      })
      .catch(error => {
        console.error(error); // "Operation failed!"
      });

### Chaining Promises

    fetch('/api/users')
      .then(response => response.json())
      .then(users => {
        console.log(users);
        return fetch(`/api/users/${users[0].id}`);
      })
      .then(response => response.json())
      .then(user => {
        console.log(user);
      })
      .catch(error => {
        console.error('Error:', error);
      });

---

## Promise Methods

### Promise.all()

Waits for all promises to resolve or any to reject:

    const promise1 = Promise.resolve(3);
    const promise2 = 42;
    const promise3 = new Promise((resolve) => {
      setTimeout(resolve, 100, 'foo');
    });
    
    Promise.all([promise1, promise2, promise3])
      .then(values => {
        console.log(values); // [3, 42, "foo"]
      })
      .catch(error => {
        console.error('One promise rejected:', error);
      });

### Promise.allSettled()

Waits for all promises to settle (fulfill or reject):

    const promise1 = Promise.resolve(3);
    const promise2 = Promise.reject('Error');
    
    Promise.allSettled([promise1, promise2])
      .then(results => {
        console.log(results);
        // [
        //   { status: 'fulfilled', value: 3 },
        //   { status: 'rejected', reason: 'Error' }
        // ]
      });

### Promise.race()

Returns the first settled promise:

    const promise1 = new Promise((resolve) => {
      setTimeout(resolve, 500, 'one');
    });
    const promise2 = new Promise((resolve) => {
      setTimeout(resolve, 100, 'two');
    });
    
    Promise.race([promise1, promise2])
      .then(value => {
        console.log(value); // "two" (faster)
      });

### Promise.any()

Returns the first fulfilled promise:

    const promise1 = Promise.reject('Error 1');
    const promise2 = Promise.resolve('Success');
    
    Promise.any([promise1, promise2])
      .then(value => {
        console.log(value); // "Success"
      });

### Promise.resolve() and Promise.reject()

    const resolved = Promise.resolve('Value');
    const rejected = Promise.reject('Error');

---

## Async/Await

### Basic Syntax

    async function fetchData() {
      try {
        const response = await fetch('/api/data');
        const data = await response.json();
        return data;
      } catch (error) {
        console.error('Error:', error);
        throw error;
      }
    }

### Async Functions Always Return Promises

    async function getValue() {
      return 'Hello';
    }
    
    getValue().then(value => {
      console.log(value); // "Hello"
    });

### Error Handling with try/catch

    async function fetchUser(id) {
      try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) {
          throw new Error('User not found');
        }
        const user = await response.json();
        return user;
      } catch (error) {
        console.error('Failed to fetch user:', error);
        throw error;
      }
    }

### Parallel Execution

    // Sequential (slow)
    async function sequential() {
      const user = await fetchUser(1);
      const posts = await fetchPosts(user.id);
      return { user, posts };
    }
    
    // Parallel (fast)
    async function parallel() {
      const [user, posts] = await Promise.all([
        fetchUser(1),
        fetchPosts(1)
      ]);
      return { user, posts };
    }

---

## Practical Examples

### Fetch API with Promises

    function fetchUserData(userId) {
      return fetch(`https://api.example.com/users/${userId}`)
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          return response.json();
        })
        .then(data => {
          console.log('User data:', data);
          return data;
        })
        .catch(error => {
          console.error('Error fetching user:', error);
          throw error;
        });
    }

### Fetch API with Async/Await

    async function fetchUserData(userId) {
      try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }
        
        const data = await response.json();
        console.log('User data:', data);
        return data;
      } catch (error) {
        console.error('Error fetching user:', error);
        throw error;
      }
    }

### Timeout Wrapper

    function withTimeout(promise, timeoutMs) {
      return Promise.race([
        promise,
        new Promise((_, reject) => {
          setTimeout(() => reject(new Error('Timeout')), timeoutMs);
        })
      ]);
    }
    
    // Usage
    withTimeout(fetch('/api/data'), 5000)
      .then(response => response.json())
      .catch(error => {
        if (error.message === 'Timeout') {
          console.error('Request timed out');
        }
      });

### Retry Logic

    async function retry(fn, retries = 3) {
      for (let i = 0; i < retries; i++) {
        try {
          return await fn();
        } catch (error) {
          if (i === retries - 1) throw error;
          await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)));
        }
      }
    }
    
    // Usage
    retry(() => fetch('/api/data'))
      .then(response => response.json())
      .then(data => console.log(data));

---

## Converting Callbacks to Promises

### Manual Conversion

    function readFilePromise(filename) {
      return new Promise((resolve, reject) => {
        fs.readFile(filename, 'utf8', (err, data) => {
          if (err) {
            reject(err);
          } else {
            resolve(data);
          }
        });
      });
    }

### Using util.promisify (Node.js)

    const util = require('util');
    const fs = require('fs');
    const readFile = util.promisify(fs.readFile);
    
    readFile('file.txt', 'utf8')
      .then(data => console.log(data))
      .catch(error => console.error(error));

---

## Advanced Patterns

### Promise Queue

    class PromiseQueue {
      constructor(concurrency = 1) {
        this.concurrency = concurrency;
        this.running = 0;
        this.queue = [];
      }
      
      add(promiseFn) {
        return new Promise((resolve, reject) => {
          this.queue.push({
            promiseFn,
            resolve,
            reject
          });
          this.process();
        });
      }
      
      async process() {
        if (this.running >= this.concurrency || this.queue.length === 0) {
          return;
        }
        
        this.running++;
        const { promiseFn, resolve, reject } = this.queue.shift();
        
        try {
          const result = await promiseFn();
          resolve(result);
        } catch (error) {
          reject(error);
        } finally {
          this.running--;
          this.process();
        }
      }
    }

### Debounce with Promises

    function debouncePromise(fn, delay) {
      let timeoutId;
      let lastPromise;
      
      return function(...args) {
        return new Promise((resolve, reject) => {
          clearTimeout(timeoutId);
          
          if (lastPromise) {
            lastPromise.catch(() => {}); // Ignore previous rejections
          }
          
          timeoutId = setTimeout(async () => {
            try {
              const result = await fn(...args);
              resolve(result);
            } catch (error) {
              reject(error);
            }
          }, delay);
        });
      };
    }

---

## Best Practices

1. **Always handle errors**: Use `.catch()` or `try/catch` with async/await
2. **Avoid promise hell**: Use async/await for cleaner code
3. **Use Promise.all() for parallel operations**: Don't await sequentially
4. **Don't forget to return**: In promise chains, return values
5. **Use Promise.allSettled()** when you need all results
6. **Handle timeouts**: Prevent hanging promises
7. **Clean up resources**: Cancel requests when possible
8. **Use proper error types**: Create custom error classes

---

## Common Mistakes

### Mistake 1: Forgetting return

    // Wrong
    promise.then(value => {
      processValue(value);
    });
    
    // Correct
    promise.then(value => {
      return processValue(value);
    });

### Mistake 2: Not handling errors

    // Wrong
    async function fetchData() {
      const data = await fetch('/api/data');
      return data.json();
    }
    
    // Correct
    async function fetchData() {
      try {
        const response = await fetch('/api/data');
        return await response.json();
      } catch (error) {
        console.error('Error:', error);
        throw error;
      }
    }

### Mistake 3: Sequential when parallel is possible

    // Wrong (slow)
    const user = await fetchUser();
    const posts = await fetchPosts();
    
    // Correct (fast)
    const [user, posts] = await Promise.all([
      fetchUser(),
      fetchPosts()
    ]);

---

## Conclusion

Promises and async/await are essential tools for modern JavaScript development. They make asynchronous code more readable and maintainable. Understanding these concepts will help you write better, more efficient JavaScript applications.

