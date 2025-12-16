# Authentication and Authorization in Node.js

## Introduction

Authentication and authorization are crucial for securing Node.js applications. This tutorial covers JWT authentication, password hashing, session management, and implementing secure authentication systems.

---

## Password Hashing with bcrypt

### Installation

    npm install bcrypt

### Hashing Passwords

    const bcrypt = require('bcrypt');
    const saltRounds = 10;
    
    // Hash password
    const hashPassword = async (password) => {
      const salt = await bcrypt.genSalt(saltRounds);
      const hash = await bcrypt.hash(password, salt);
      return hash;
    };
    
    // Verify password
    const verifyPassword = async (password, hash) => {
      return await bcrypt.compare(password, hash);
    };
    
    // Usage
    const hashed = await hashPassword('mypassword');
    const isValid = await verifyPassword('mypassword', hashed);

---

## JWT Authentication

### Installation

    npm install jsonwebtoken

### Creating Tokens

    const jwt = require('jsonwebtoken');
    const SECRET_KEY = process.env.JWT_SECRET || 'your-secret-key';
    
    // Generate token
    const generateToken = (userId) => {
      return jwt.sign(
        { userId },
        SECRET_KEY,
        { expiresIn: '24h' }
      );
    };
    
    // Verify token
    const verifyToken = (token) => {
      try {
        return jwt.verify(token, SECRET_KEY);
      } catch (error) {
        throw new Error('Invalid token');
      }
    };

### Authentication Middleware

    const authenticateToken = (req, res, next) => {
      const authHeader = req.headers['authorization'];
      const token = authHeader && authHeader.split(' ')[1]; // Bearer TOKEN
      
      if (!token) {
        return res.status(401).json({
          success: false,
          error: 'Access token required'
        });
      }
      
      try {
        const decoded = jwt.verify(token, SECRET_KEY);
        req.user = decoded;
        next();
      } catch (error) {
        res.status(403).json({
          success: false,
          error: 'Invalid or expired token'
        });
      }
    };
    
    // Usage
    app.get('/api/protected', authenticateToken, (req, res) => {
      res.json({ message: 'Protected route', user: req.user });
    });

---

## Complete Authentication System

### User Registration

    const express = require('express');
    const bcrypt = require('bcrypt');
    const jwt = require('jsonwebtoken');
    const app = express();
    
    app.use(express.json());
    
    let users = [];
    
    // Register
    app.post('/api/register', async (req, res) => {
      try {
        const { email, password, name } = req.body;
        
        // Validation
        if (!email || !password || !name) {
          return res.status(400).json({
            success: false,
            error: 'All fields are required'
          });
        }
        
        // Check if user exists
        if (users.find(u => u.email === email)) {
          return res.status(400).json({
            success: false,
            error: 'User already exists'
          });
        }
        
        // Hash password
        const hashedPassword = await bcrypt.hash(password, 10);
        
        // Create user
        const user = {
          id: users.length + 1,
          email,
          name,
          password: hashedPassword
        };
        
        users.push(user);
        
        // Generate token
        const token = jwt.sign(
          { userId: user.id, email: user.email },
          process.env.JWT_SECRET || 'secret',
          { expiresIn: '24h' }
        );
        
        res.status(201).json({
          success: true,
          data: {
            user: { id: user.id, email: user.email, name: user.name },
            token
          }
        });
      } catch (error) {
        res.status(500).json({
          success: false,
          error: 'Registration failed'
        });
      }
    });

### User Login

    app.post('/api/login', async (req, res) => {
      try {
        const { email, password } = req.body;
        
        if (!email || !password) {
          return res.status(400).json({
            success: false,
            error: 'Email and password are required'
          });
        }
        
        // Find user
        const user = users.find(u => u.email === email);
        if (!user) {
          return res.status(401).json({
            success: false,
            error: 'Invalid credentials'
          });
        }
        
        // Verify password
        const isValidPassword = await bcrypt.compare(password, user.password);
        if (!isValidPassword) {
          return res.status(401).json({
            success: false,
            error: 'Invalid credentials'
          });
        }
        
        // Generate token
        const token = jwt.sign(
          { userId: user.id, email: user.email },
          process.env.JWT_SECRET || 'secret',
          { expiresIn: '24h' }
        );
        
        res.json({
          success: true,
          data: {
            user: { id: user.id, email: user.email, name: user.name },
            token
          }
        });
      } catch (error) {
        res.status(500).json({
          success: false,
          error: 'Login failed'
        });
      }
    });

---

## Authorization (Role-based)

### Role-based Middleware

    const authorize = (...roles) => {
      return (req, res, next) => {
        if (!req.user) {
          return res.status(401).json({
            success: false,
            error: 'Authentication required'
          });
        }
        
        if (!roles.includes(req.user.role)) {
          return res.status(403).json({
            success: false,
            error: 'Insufficient permissions'
          });
        }
        
        next();
      };
    };
    
    // Usage
    app.delete('/api/users/:id',
      authenticateToken,
      authorize('admin'),
      (req, res) => {
        // Delete user
      }
    );

---

## Refresh Tokens

    // Generate refresh token
    const generateRefreshToken = (userId) => {
      return jwt.sign(
        { userId, type: 'refresh' },
        process.env.REFRESH_SECRET || 'refresh-secret',
        { expiresIn: '7d' }
      );
    };
    
    // Refresh access token
    app.post('/api/refresh', (req, res) => {
      const { refreshToken } = req.body;
      
      if (!refreshToken) {
        return res.status(401).json({
          success: false,
          error: 'Refresh token required'
        });
      }
      
      try {
        const decoded = jwt.verify(
          refreshToken,
          process.env.REFRESH_SECRET || 'refresh-secret'
        );
        
        if (decoded.type !== 'refresh') {
          throw new Error('Invalid token type');
        }
        
        const newAccessToken = jwt.sign(
          { userId: decoded.userId },
          process.env.JWT_SECRET || 'secret',
          { expiresIn: '15m' }
        );
        
        res.json({
          success: true,
          data: { token: newAccessToken }
        });
      } catch (error) {
        res.status(403).json({
          success: false,
          error: 'Invalid refresh token'
        });
      }
    });

---

## Session-based Authentication

### Using express-session

    const session = require('express-session');
    
    app.use(session({
      secret: process.env.SESSION_SECRET || 'secret',
      resave: false,
      saveUninitialized: false,
      cookie: {
        secure: process.env.NODE_ENV === 'production',
        httpOnly: true,
        maxAge: 24 * 60 * 60 * 1000 // 24 hours
      }
    }));
    
    // Login with session
    app.post('/api/login', async (req, res) => {
      const { email, password } = req.body;
      const user = await validateUser(email, password);
      
      if (user) {
        req.session.userId = user.id;
        res.json({ success: true, user });
      } else {
        res.status(401).json({ success: false, error: 'Invalid credentials' });
      }
    });
    
    // Protected route
    app.get('/api/profile', (req, res) => {
      if (!req.session.userId) {
        return res.status(401).json({ success: false, error: 'Not authenticated' });
      }
      // Get user data
    });
    
    // Logout
    app.post('/api/logout', (req, res) => {
      req.session.destroy();
      res.json({ success: true, message: 'Logged out' });
    });

---

## OAuth Integration

### Passport.js Setup

    const passport = require('passport');
    const GoogleStrategy = require('passport-google-oauth20').Strategy;
    
    passport.use(new GoogleStrategy({
      clientID: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
      callbackURL: '/api/auth/google/callback'
    }, async (accessToken, refreshToken, profile, done) => {
      // Find or create user
      let user = await User.findOne({ googleId: profile.id });
      if (!user) {
        user = await User.create({
          googleId: profile.id,
          email: profile.emails[0].value,
          name: profile.displayName
        });
      }
      return done(null, user);
    }));
    
    app.get('/api/auth/google',
      passport.authenticate('google', { scope: ['profile', 'email'] })
    );
    
    app.get('/api/auth/google/callback',
      passport.authenticate('google', { failureRedirect: '/login' }),
      (req, res) => {
        const token = generateToken(req.user.id);
        res.redirect(`/dashboard?token=${token}`);
      }
    );

---

## Security Best Practices

1. **Use HTTPS** in production
2. **Store secrets** in environment variables
3. **Hash passwords** with bcrypt (never store plain text)
4. **Use strong JWT secrets**
5. **Set appropriate token expiration**
6. **Implement rate limiting** on auth endpoints
7. **Validate input** on all endpoints
8. **Use httpOnly cookies** for tokens when possible
9. **Implement CSRF protection**
10. **Log authentication attempts**

---

## Conclusion

Implementing proper authentication and authorization is essential for secure applications. Use JWT for stateless authentication, hash passwords properly, and implement role-based access control for authorization.

