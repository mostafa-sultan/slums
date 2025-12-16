# Error Handling in Node.js

## Introduction

Proper error handling is crucial for robust Node.js applications. This tutorial covers error handling patterns, custom errors, async error handling, and best practices.

---

## Try-Catch Blocks

    try {
      // Code that might throw error
      const result = riskyOperation();
    } catch (error) {
      console.error('Error occurred:', error.message);
      // Handle error
    }

---

## Custom Error Classes

    class AppError extends Error {
      constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = true;
        Error.captureStackTrace(this, this.constructor);
      }
    }
    
    // Usage
    throw new AppError('User not found', 404);

---

## Async Error Handling

    // Using try-catch with async/await
    async function fetchData() {
      try {
        const data = await someAsyncOperation();
        return data;
      } catch (error) {
        console.error('Error:', error);
        throw error;
      }
    }
    
    // Using .catch() with promises
    fetchData()
      .then(data => console.log(data))
      .catch(error => console.error('Error:', error));

---

## Express Error Handling

    // Error handling middleware (must be last)
    app.use((err, req, res, next) => {
      const statusCode = err.statusCode || 500;
      const message = err.message || 'Internal Server Error';
      
      res.status(statusCode).json({
        success: false,
        error: message,
        ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
      });
    });
    
    // Usage in routes
    app.get('/api/users/:id', async (req, res, next) => {
      try {
        const user = await User.findById(req.params.id);
        if (!user) {
          throw new AppError('User not found', 404);
        }
        res.json({ success: true, data: user });
      } catch (error) {
        next(error);
      }
    });

---

## Async Handler Wrapper

    const asyncHandler = (fn) => {
      return (req, res, next) => {
        Promise.resolve(fn(req, res, next)).catch(next);
      };
    };
    
    // Usage
    app.get('/api/users/:id', asyncHandler(async (req, res) => {
      const user = await User.findById(req.params.id);
      if (!user) {
        throw new AppError('User not found', 404);
      }
      res.json({ success: true, data: user });
    }));

---

## Error Types

    class ValidationError extends AppError {
      constructor(message) {
        super(message, 400);
      }
    }
    
    class NotFoundError extends AppError {
      constructor(resource) {
        super(`${resource} not found`, 404);
      }
    }
    
    class UnauthorizedError extends AppError {
      constructor(message = 'Unauthorized') {
        super(message, 401);
      }
    }

---

## Best Practices

1. **Always handle errors** in async operations
2. **Use custom error classes** for different error types
3. **Provide meaningful error messages**
4. **Log errors** for debugging
5. **Don't expose sensitive information** in error messages
6. **Use error handling middleware** in Express
7. **Handle unhandled promise rejections**
8. **Handle uncaught exceptions**

---

## Conclusion

Proper error handling makes applications more robust and maintainable. Use try-catch, custom error classes, and error handling middleware for comprehensive error management.

