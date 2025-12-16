# Express Middleware in Node.js

## Introduction

Middleware functions are the heart of Express.js applications. They have access to the request object (req), response object (res), and the next middleware function in the application's request-response cycle. This tutorial covers everything about middleware.

---

## What is Middleware?

Middleware functions can:
- Execute any code
- Make changes to request and response objects
- End the request-response cycle
- Call the next middleware in the stack

### Basic Middleware

    const express = require('express');
    const app = express();
    
    // Simple middleware
    const myMiddleware = (req, res, next) => {
      console.log('Middleware executed');
      next(); // Pass control to next middleware
    };
    
    app.use(myMiddleware);
    
    app.get('/', (req, res) => {
      res.send('Hello World');
    });

---

## Types of Middleware

### Application-level Middleware

Applied to all routes:

    app.use((req, res, next) => {
      console.log('Time:', Date.now());
      next();
    });
    
    // Multiple middleware
    app.use('/user/:id', (req, res, next) => {
      console.log('Request Type:', req.method);
      next();
    }, (req, res, next) => {
      console.log('Request URL:', req.originalUrl);
      next();
    });

### Router-level Middleware

Applied to specific routes:

    const router = express.Router();
    
    router.use((req, res, next) => {
      console.log('Router middleware');
      next();
    });
    
    router.get('/users', (req, res) => {
      res.send('Users');
    });

### Error-handling Middleware

Must have 4 parameters:

    app.use((err, req, res, next) => {
      console.error(err.stack);
      res.status(500).send('Something broke!');
    });

### Built-in Middleware

    // Parse JSON bodies
    app.use(express.json());
    
    // Parse URL-encoded bodies
    app.use(express.urlencoded({ extended: true }));
    
    // Serve static files
    app.use(express.static('public'));

### Third-party Middleware

    const morgan = require('morgan');
    const helmet = require('helmet');
    const cors = require('cors');
    
    app.use(morgan('combined'));
    app.use(helmet());
    app.use(cors());

---

## Common Middleware Examples

### Logger Middleware

    const logger = (req, res, next) => {
      const timestamp = new Date().toISOString();
      console.log(`${timestamp} - ${req.method} ${req.path}`);
      next();
    };
    
    app.use(logger);

### Authentication Middleware

    const authenticate = (req, res, next) => {
      const token = req.headers.authorization;
      
      if (!token) {
        return res.status(401).json({
          success: false,
          error: 'No token provided'
        });
      }
      
      try {
        // Verify token
        const decoded = jwt.verify(token, 'secret');
        req.user = decoded;
        next();
      } catch (error) {
        res.status(401).json({
          success: false,
          error: 'Invalid token'
        });
      }
    };
    
    // Use on protected routes
    app.get('/api/protected', authenticate, (req, res) => {
      res.json({ message: 'Protected data', user: req.user });
    });

### Validation Middleware

    const validateUser = (req, res, next) => {
      const { name, email } = req.body;
      
      if (!name || !email) {
        return res.status(400).json({
          success: false,
          error: 'Name and email are required'
        });
      }
      
      if (!email.includes('@')) {
        return res.status(400).json({
          success: false,
          error: 'Invalid email format'
        });
      }
      
      next();
    };
    
    app.post('/api/users', validateUser, (req, res) => {
      // Create user
    });

### Rate Limiting Middleware

    const rateLimit = require('express-rate-limit');
    
    const limiter = rateLimit({
      windowMs: 15 * 60 * 1000, // 15 minutes
      max: 100 // limit each IP to 100 requests per windowMs
    });
    
    app.use('/api/', limiter);

### CORS Middleware

    const cors = require('cors');
    
    const corsOptions = {
      origin: 'https://example.com',
      methods: ['GET', 'POST', 'PUT', 'DELETE'],
      allowedHeaders: ['Content-Type', 'Authorization']
    };
    
    app.use(cors(corsOptions));

---

## Custom Middleware Patterns

### Request Timing

    const requestTime = (req, res, next) => {
      req.requestTime = Date.now();
      next();
    };
    
    app.use(requestTime);
    
    app.get('/', (req, res) => {
      res.send(`Request received at ${req.requestTime}`);
    });

### Request ID

    const requestId = (req, res, next) => {
      req.id = Math.random().toString(36).substring(7);
      next();
    };
    
    app.use(requestId);

### Body Parser (Custom)

    const bodyParser = (req, res, next) => {
      let data = '';
      
      req.on('data', chunk => {
        data += chunk;
      });
      
      req.on('end', () => {
        try {
          req.body = JSON.parse(data);
          next();
        } catch (error) {
          res.status(400).json({ error: 'Invalid JSON' });
        }
      });
    };

---

## Middleware Execution Order

    app.use((req, res, next) => {
      console.log('1. First middleware');
      next();
    });
    
    app.use((req, res, next) => {
      console.log('2. Second middleware');
      next();
    });
    
    app.get('/', (req, res) => {
      console.log('3. Route handler');
      res.send('Done');
    });
    
    // Output: 1, 2, 3

---

## Conditional Middleware

    const conditionalMiddleware = (condition) => {
      return (req, res, next) => {
        if (condition) {
          // Execute middleware logic
          console.log('Condition met');
        }
        next();
      };
    };
    
    app.use(conditionalMiddleware(process.env.NODE_ENV === 'development'));

---

## Error Handling Middleware

### Basic Error Handler

    app.use((err, req, res, next) => {
      console.error(err.stack);
      res.status(500).json({
        success: false,
        error: 'Internal Server Error'
      });
    });

### Custom Error Handler

    class AppError extends Error {
      constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
      }
    }
    
    const errorHandler = (err, req, res, next) => {
      const statusCode = err.statusCode || 500;
      const message = err.message || 'Internal Server Error';
      
      res.status(statusCode).json({
        success: false,
        error: message,
        ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
      });
    };
    
    app.use(errorHandler);
    
    // Usage
    app.get('/api/users/:id', (req, res, next) => {
      const user = findUser(req.params.id);
      if (!user) {
        return next(new AppError('User not found', 404));
      }
      res.json({ success: true, data: user });
    });

---

## Async Middleware

### Handling Async Errors

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

## Middleware Best Practices

1. **Always call next()** unless ending the request
2. **Handle errors properly** with error middleware
3. **Use async/await** with proper error handling
4. **Order matters** - place middleware in correct order
5. **Reuse middleware** - create reusable functions
6. **Use third-party middleware** when appropriate
7. **Document custom middleware**
8. **Test middleware** independently

---

## Common Middleware Libraries

### Morgan (Logging)

    const morgan = require('morgan');
    app.use(morgan('combined'));

### Helmet (Security)

    const helmet = require('helmet');
    app.use(helmet());

### Compression

    const compression = require('compression');
    app.use(compression());

### Cookie Parser

    const cookieParser = require('cookie-parser');
    app.use(cookieParser());

### Express Validator

    const { body, validationResult } = require('express-validator');
    
    app.post('/api/users',
      body('email').isEmail(),
      body('password').isLength({ min: 6 }),
      (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
          return res.status(400).json({ errors: errors.array() });
        }
        // Process request
      }
    );

---

## Conclusion

Middleware is essential for Express.js applications. Understanding how to create, use, and organize middleware will help you build robust and maintainable Node.js applications.

