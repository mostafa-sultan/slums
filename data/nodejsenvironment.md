# Environment Variables and Configuration in Node.js

## Introduction

Managing environment variables and configuration is essential for Node.js applications. This tutorial covers using dotenv, environment-specific configs, and best practices.

---

## Using dotenv

### Installation

    npm install dotenv

### Basic Usage

    require('dotenv').config();
    
    const port = process.env.PORT || 3000;
    const dbUrl = process.env.DATABASE_URL;
    
    console.log(`Server running on port ${port}`);

### .env File

    PORT=3000
    DATABASE_URL=mongodb://localhost:27017/mydb
    JWT_SECRET=your-secret-key
    NODE_ENV=development

### Accessing Variables

    const express = require('express');
    require('dotenv').config();
    
    const app = express();
    const PORT = process.env.PORT || 3000;
    
    app.get('/', (req, res) => {
      res.json({
        environment: process.env.NODE_ENV,
        port: PORT
      });
    });
    
    app.listen(PORT, () => {
      console.log(`Server running on port ${PORT}`);
    });

---

## Configuration Management

### Config Module

    // config/index.js
    require('dotenv').config();
    
    module.exports = {
      port: process.env.PORT || 3000,
      db: {
        url: process.env.DATABASE_URL,
        options: {
          useNewUrlParser: true,
          useUnifiedTopology: true
        }
      },
      jwt: {
        secret: process.env.JWT_SECRET,
        expiresIn: '24h'
      },
      environment: process.env.NODE_ENV || 'development'
    };
    
    // Usage
    const config = require('./config');
    console.log(config.port);

---

## Environment-Specific Configs

    // config/development.js
    module.exports = {
      port: 3000,
      db: 'mongodb://localhost:27017/devdb'
    };
    
    // config/production.js
    module.exports = {
      port: process.env.PORT,
      db: process.env.DATABASE_URL
    };
    
    // config/index.js
    const env = process.env.NODE_ENV || 'development';
    const config = require(`./${env}`);
    module.exports = config;

---

## Best Practices

1. **Never commit .env files** to version control
2. **Use .env.example** as a template
3. **Validate required variables** on startup
4. **Use different files** for different environments
5. **Store secrets** securely
6. **Use default values** when appropriate

---

## Conclusion

Proper configuration management is crucial for maintainable applications. Use dotenv for local development and environment variables for production.

