# Building REST APIs with Node.js and Express

## Introduction

REST (Representational State Transfer) is an architectural style for designing web services. This tutorial covers building RESTful APIs using Node.js and Express.js, including routing, middleware, error handling, and best practices.

---

## What is REST?

REST is a set of principles for designing web services:
- **Stateless**: Each request contains all information needed
- **Resource-based**: URLs represent resources
- **HTTP Methods**: GET, POST, PUT, DELETE, PATCH
- **JSON**: Common data format

---

## Setting Up Express Server

### Basic Setup

    const express = require('express');
    const app = express();
    const PORT = process.env.PORT || 3000;
    
    // Middleware
    app.use(express.json());
    app.use(express.urlencoded({ extended: true }));
    
    // Routes
    app.get('/', (req, res) => {
      res.json({ message: 'Welcome to the API' });
    });
    
    app.listen(PORT, () => {
      console.log(`Server running on port ${PORT}`);
    });

---

## RESTful Routes

### Resource Naming

    // Good
    GET    /api/users
    GET    /api/users/:id
    POST   /api/users
    PUT    /api/users/:id
    DELETE /api/users/:id
    
    // Bad
    GET    /api/getUsers
    POST   /api/createUser
    GET    /api/user/:id/delete

### HTTP Methods

- **GET**: Retrieve data
- **POST**: Create new resource
- **PUT**: Update entire resource
- **PATCH**: Partial update
- **DELETE**: Remove resource

---

## Complete REST API Example

### User Model (In-memory)

    let users = [
      { id: 1, name: 'John Doe', email: 'john@example.com' },
      { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
    ];
    let nextId = 3;

### GET All Users

    app.get('/api/users', (req, res) => {
      res.json({
        success: true,
        count: users.length,
        data: users
      });
    });

### GET Single User

    app.get('/api/users/:id', (req, res) => {
      const id = parseInt(req.params.id);
      const user = users.find(u => u.id === id);
      
      if (!user) {
        return res.status(404).json({
          success: false,
          error: 'User not found'
        });
      }
      
      res.json({
        success: true,
        data: user
      });
    });

### POST Create User

    app.post('/api/users', (req, res) => {
      const { name, email } = req.body;
      
      // Validation
      if (!name || !email) {
        return res.status(400).json({
          success: false,
          error: 'Please provide name and email'
        });
      }
      
      const newUser = {
        id: nextId++,
        name,
        email
      };
      
      users.push(newUser);
      
      res.status(201).json({
        success: true,
        data: newUser
      });
    });

### PUT Update User

    app.put('/api/users/:id', (req, res) => {
      const id = parseInt(req.params.id);
      const userIndex = users.findIndex(u => u.id === id);
      
      if (userIndex === -1) {
        return res.status(404).json({
          success: false,
          error: 'User not found'
        });
      }
      
      const { name, email } = req.body;
      users[userIndex] = {
        ...users[userIndex],
        name: name || users[userIndex].name,
        email: email || users[userIndex].email
      };
      
      res.json({
        success: true,
        data: users[userIndex]
      });
    });

### DELETE User

    app.delete('/api/users/:id', (req, res) => {
      const id = parseInt(req.params.id);
      const userIndex = users.findIndex(u => u.id === id);
      
      if (userIndex === -1) {
        return res.status(404).json({
          success: false,
          error: 'User not found'
        });
      }
      
      users.splice(userIndex, 1);
      
      res.status(204).send();
    });

---

## Organizing Routes

### Using Express Router

    // routes/users.js
    const express = require('express');
    const router = express.Router();
    
    router.get('/', (req, res) => {
      // Get all users
    });
    
    router.get('/:id', (req, res) => {
      // Get single user
    });
    
    router.post('/', (req, res) => {
      // Create user
    });
    
    router.put('/:id', (req, res) => {
      // Update user
    });
    
    router.delete('/:id', (req, res) => {
      // Delete user
    });
    
    module.exports = router;
    
    // app.js
    const usersRouter = require('./routes/users');
    app.use('/api/users', usersRouter);

---

## Middleware

### Custom Middleware

    // Logger middleware
    const logger = (req, res, next) => {
      console.log(`${req.method} ${req.path} - ${new Date().toISOString()}`);
      next();
    };
    
    app.use(logger);
    
    // Authentication middleware
    const authenticate = (req, res, next) => {
      const token = req.headers.authorization;
      
      if (!token) {
        return res.status(401).json({
          success: false,
          error: 'No token provided'
        });
      }
      
      // Verify token
      req.user = { id: 1, name: 'John' };
      next();
    };
    
    // Use on specific routes
    app.get('/api/protected', authenticate, (req, res) => {
      res.json({ message: 'Protected route', user: req.user });
    });

### Error Handling Middleware

    const errorHandler = (err, req, res, next) => {
      console.error(err.stack);
      
      res.status(err.status || 500).json({
        success: false,
        error: err.message || 'Internal Server Error'
      });
    };
    
    app.use(errorHandler);

---

## Validation

### Using express-validator

    const { body, validationResult } = require('express-validator');
    
    app.post('/api/users',
      body('name').trim().isLength({ min: 2 }).withMessage('Name must be at least 2 characters'),
      body('email').isEmail().withMessage('Invalid email'),
      (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
          return res.status(400).json({
            success: false,
            errors: errors.array()
          });
        }
        
        // Process request
      }
    );

---

## Query Parameters and Pagination

    app.get('/api/users', (req, res) => {
      const page = parseInt(req.query.page) || 1;
      const limit = parseInt(req.query.limit) || 10;
      const search = req.query.search || '';
      
      let filteredUsers = users;
      
      // Search
      if (search) {
        filteredUsers = users.filter(u => 
          u.name.toLowerCase().includes(search.toLowerCase())
        );
      }
      
      // Pagination
      const startIndex = (page - 1) * limit;
      const endIndex = page * limit;
      const paginatedUsers = filteredUsers.slice(startIndex, endIndex);
      
      res.json({
        success: true,
        pagination: {
          page,
          limit,
          total: filteredUsers.length,
          pages: Math.ceil(filteredUsers.length / limit)
        },
        data: paginatedUsers
      });
    });

---

## Error Handling

### Custom Error Class

    class AppError extends Error {
      constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = true;
        Error.captureStackTrace(this, this.constructor);
      }
    }
    
    // Usage
    app.get('/api/users/:id', (req, res, next) => {
      const user = users.find(u => u.id === parseInt(req.params.id));
      
      if (!user) {
        return next(new AppError('User not found', 404));
      }
      
      res.json({ success: true, data: user });
    });

---

## CORS

    const cors = require('cors');
    
    // Allow all origins
    app.use(cors());
    
    // Configure CORS
    app.use(cors({
      origin: 'https://example.com',
      methods: ['GET', 'POST', 'PUT', 'DELETE'],
      allowedHeaders: ['Content-Type', 'Authorization']
    }));

---

## Best Practices

1. **Use proper HTTP status codes**
2. **Validate input data**
3. **Handle errors consistently**
4. **Use middleware for common tasks**
5. **Organize routes with Router**
6. **Implement pagination for large datasets**
7. **Use environment variables**
8. **Add request logging**
9. **Implement rate limiting**
10. **Use HTTPS in production**

---

## Complete Example

    const express = require('express');
    const app = express();
    
    app.use(express.json());
    
    let users = [];
    let nextId = 1;
    
    // GET all users
    app.get('/api/users', (req, res) => {
      res.json({ success: true, data: users });
    });
    
    // GET single user
    app.get('/api/users/:id', (req, res) => {
      const user = users.find(u => u.id === parseInt(req.params.id));
      if (!user) {
        return res.status(404).json({ success: false, error: 'Not found' });
      }
      res.json({ success: true, data: user });
    });
    
    // POST create user
    app.post('/api/users', (req, res) => {
      const { name, email } = req.body;
      if (!name || !email) {
        return res.status(400).json({ success: false, error: 'Invalid data' });
      }
      const user = { id: nextId++, name, email };
      users.push(user);
      res.status(201).json({ success: true, data: user });
    });
    
    // PUT update user
    app.put('/api/users/:id', (req, res) => {
      const id = parseInt(req.params.id);
      const userIndex = users.findIndex(u => u.id === id);
      if (userIndex === -1) {
        return res.status(404).json({ success: false, error: 'Not found' });
      }
      users[userIndex] = { ...users[userIndex], ...req.body };
      res.json({ success: true, data: users[userIndex] });
    });
    
    // DELETE user
    app.delete('/api/users/:id', (req, res) => {
      const id = parseInt(req.params.id);
      users = users.filter(u => u.id !== id);
      res.status(204).send();
    });
    
    const PORT = 3000;
    app.listen(PORT, () => {
      console.log(`Server running on port ${PORT}`);
    });

---

## Conclusion

Building REST APIs with Express is straightforward. Follow REST principles, organize your code well, handle errors properly, and your API will be maintainable and scalable.

