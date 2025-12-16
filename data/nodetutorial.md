# Node.js Tutorial

## Introduction

Node.js is built on the V8 JavaScript runtime and is designed to be lightweight and efficient. It uses an event-driven, non-blocking I/O model that makes it well-suited for building real-time applications, APIs, and microservices.

### Key Features of Node.js

1. **Asynchronous I/O**: Node.js is designed to handle many connections simultaneously without blocking the execution of code, making it highly performant.
2. **NPM (Node Package Manager)**: NPM is the package manager for Node.js, providing a vast ecosystem of open-source libraries and tools that can be easily integrated into Node.js projects.
3. **Single-threaded Event Loop**: Node.js employs a single-threaded event loop to handle multiple concurrent requests. This allows for efficient handling of I/O operations without creating separate threads for each connection.
4. **Cross-platform**: Node.js is cross-platform and can run on various operating systems, making it versatile for different deployment environments.
5. **Fast Execution**: Built on Chrome's V8 JavaScript engine, Node.js executes JavaScript code very quickly.
6. **Rich Ecosystem**: Access to thousands of packages through NPM.

---

## Installation

### Installing Node.js

1. **Download from official website**: Visit [nodejs.org](https://nodejs.org) and download the LTS (Long Term Support) version
2. **Using package managers**:
   - macOS: `brew install node`
   - Ubuntu/Debian: `sudo apt-get install nodejs npm`
   - Windows: Use the installer from the official website

### Verifying Installation

    node --version    // Check Node.js version
    npm --version     // Check NPM version

---

## Setting Up a Node.js Project

### Initializing a Node.js Project

    npm init -y

This creates a `package.json` file with default values. You can also use `npm init` for interactive setup.

### Installing Dependencies

    // Install a package
    npm install express
    
    // Install as dev dependency
    npm install --save-dev nodemon
    
    // Install globally
    npm install -g nodemon
    
    // Install specific version
    npm install express@4.18.0

### package.json Structure

    {
      "name": "my-app",
      "version": "1.0.0",
      "description": "My Node.js application",
      "main": "index.js",
      "scripts": {
        "start": "node index.js",
        "dev": "nodemon index.js"
      },
      "dependencies": {
        "express": "^4.18.0"
      }
    }

---

## Core Modules

Node.js comes with built-in modules that don't require installation.

### File System (fs)

    const fs = require('fs');
    
    // Read file synchronously
    const data = fs.readFileSync('file.txt', 'utf8');
    
    // Read file asynchronously
    fs.readFile('file.txt', 'utf8', (err, data) => {
      if (err) {
        console.error(err);
        return;
      }
      console.log(data);
    });
    
    // Using Promises (fs.promises)
    const fsPromises = require('fs').promises;
    async function readFile() {
      try {
        const data = await fsPromises.readFile('file.txt', 'utf8');
        console.log(data);
      } catch (err) {
        console.error(err);
      }
    }
    
    // Write file
    fs.writeFile('file.txt', 'Hello World', (err) => {
      if (err) console.error(err);
    });
    
    // Append to file
    fs.appendFile('file.txt', '\nNew line', (err) => {
      if (err) console.error(err);
    });
    
    // Check if file exists
    fs.existsSync('file.txt');  // true or false
    
    // Create directory
    fs.mkdir('new-folder', { recursive: true }, (err) => {
      if (err) console.error(err);
    });
    
    // Read directory
    fs.readdir('.', (err, files) => {
      if (err) console.error(err);
      console.log(files);
    });

### Path Module

    const path = require('path');
    
    path.join('/users', 'john', 'documents', 'file.txt');
    // '/users/john/documents/file.txt'
    
    path.resolve('file.txt');
    // Returns absolute path
    
    path.dirname('/users/john/file.txt');
    // '/users/john'
    
    path.basename('/users/john/file.txt');
    // 'file.txt'
    
    path.extname('file.txt');
    // '.txt'
    
    path.parse('/users/john/file.txt');
    // { root: '/', dir: '/users/john', base: 'file.txt', ext: '.txt', name: 'file' }

### HTTP Module

    const http = require('http');
    
    const server = http.createServer((req, res) => {
      res.writeHead(200, { 'Content-Type': 'text/plain' });
      res.end('Hello World');
    });
    
    server.listen(3000, () => {
      console.log('Server running on port 3000');
    });

### URL Module

    const url = require('url');
    
    const myURL = new URL('https://example.com/path?name=john&age=30');
    console.log(myURL.hostname);  // 'example.com'
    console.log(myURL.pathname);  // '/path'
    console.log(myURL.searchParams.get('name'));  // 'john'
    
    // Parse URL
    const parsed = url.parse('https://example.com/path?name=john');
    console.log(parsed.hostname);  // 'example.com'

### Events Module

    const EventEmitter = require('events');
    
    class MyEmitter extends EventEmitter {}
    
    const myEmitter = new MyEmitter();
    
    // Listen for event
    myEmitter.on('event', (data) => {
      console.log('Event received:', data);
    });
    
    // Emit event
    myEmitter.emit('event', 'Hello');
    
    // Once - listen only once
    myEmitter.once('event', () => {
      console.log('This will only fire once');
    });
    
    // Remove listener
    myEmitter.removeListener('event', handler);
    myEmitter.removeAllListeners('event');

### Streams

    const fs = require('fs');
    
    // Readable stream
    const readStream = fs.createReadStream('large-file.txt');
    
    readStream.on('data', (chunk) => {
      console.log('Received chunk:', chunk.length);
    });
    
    readStream.on('end', () => {
      console.log('Finished reading');
    });
    
    // Writable stream
    const writeStream = fs.createWriteStream('output.txt');
    writeStream.write('Hello');
    writeStream.write(' World');
    writeStream.end();
    
    // Piping streams
    readStream.pipe(writeStream);

### Process Module

    process.argv;           // Command line arguments
    process.env;            // Environment variables
    process.cwd();          // Current working directory
    process.exit(0);        // Exit process
    process.on('exit', () => {
      console.log('Process exiting');
    });

---

## Express.js

Express.js is a popular web application framework for Node.js that simplifies the process of building robust and scalable web applications.

### Installation

    npm install express

### Basic Express Server

    // index.js
    const express = require('express');
    const app = express();
    const port = 3000;
    
    app.get('/', (req, res) => {
      res.send('Hello, Node.js!');
    });
    
    app.listen(port, () => {
      console.log(`Server is running on http://localhost:${port}`);
    });

### Key Features of Express.js

1. **Routing**: Express provides a straightforward mechanism for defining routes and handling HTTP requests.
2. **Middleware**: Middleware functions in Express allow you to execute code during the request-response cycle. This can be used for tasks such as authentication, logging, and error handling.
3. **Template Engines**: Express supports various template engines, such as EJS and Handlebars, for rendering dynamic content on the server.
4. **RESTful APIs**: Express is commonly used to build RESTful APIs, making it a popular choice for developing backend services.

### Express Routing

    // Basic routes
    app.get('/', (req, res) => {
      res.send('GET request');
    });
    
    app.post('/', (req, res) => {
      res.send('POST request');
    });
    
    app.put('/user/:id', (req, res) => {
      res.send(`PUT request for user ${req.params.id}`);
    });
    
    app.delete('/user/:id', (req, res) => {
      res.send(`DELETE request for user ${req.params.id}`);
    });
    
    // Route parameters
    app.get('/users/:userId/posts/:postId', (req, res) => {
      res.json({
        userId: req.params.userId,
        postId: req.params.postId
      });
    });
    
    // Query parameters
    app.get('/search', (req, res) => {
      res.json({
        query: req.query.q,
        page: req.query.page
      });
    });
    
    // Route handlers
    app.get('/example', 
      (req, res, next) => {
        console.log('First handler');
        next();
      },
      (req, res) => {
        res.send('Second handler');
      }
    );
    
    // Route with regex
    app.get(/^\/users\/(\d+)$/, (req, res) => {
      res.send(`User ID: ${req.params[0]}`);
    });

### Express Middleware

    // Built-in middleware
    app.use(express.json());        // Parse JSON bodies
    app.use(express.urlencoded({ extended: true }));  // Parse URL-encoded bodies
    app.use(express.static('public'));  // Serve static files
    
    // Custom middleware
    const logger = (req, res, next) => {
      console.log(`${req.method} ${req.path}`);
      next();
    };
    app.use(logger);
    
    // Middleware for specific route
    app.use('/api', logger);
    
    // Error handling middleware
    app.use((err, req, res, next) => {
      console.error(err.stack);
      res.status(500).send('Something broke!');
    });
    
    // Third-party middleware
    const cors = require('cors');
    app.use(cors());
    
    const helmet = require('helmet');
    app.use(helmet());

### Express Response Methods

    res.send('Hello');                    // Send response
    res.json({ name: 'John' });          // Send JSON
    res.status(404).send('Not Found');    // Set status and send
    res.redirect('/new-path');            // Redirect
    res.render('view', { data });         // Render template
    res.sendFile('/path/to/file.html');   // Send file
    res.download('/path/to/file.pdf');    // Download file
    res.set('Content-Type', 'text/plain'); // Set header
    res.cookie('name', 'value');          // Set cookie

### Express Router

    // routes/users.js
    const express = require('express');
    const router = express.Router();
    
    router.get('/', (req, res) => {
      res.json({ users: [] });
    });
    
    router.get('/:id', (req, res) => {
      res.json({ userId: req.params.id });
    });
    
    module.exports = router;
    
    // app.js
    const usersRouter = require('./routes/users');
    app.use('/users', usersRouter);

### Complete Express Example

    const express = require('express');
    const app = express();
    
    // Middleware
    app.use(express.json());
    
    // In-memory data store
    let users = [
      { id: 1, name: 'John', email: 'john@example.com' },
      { id: 2, name: 'Jane', email: 'jane@example.com' }
    ];
    
    // GET all users
    app.get('/api/users', (req, res) => {
      res.json(users);
    });
    
    // GET user by ID
    app.get('/api/users/:id', (req, res) => {
      const user = users.find(u => u.id === parseInt(req.params.id));
      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }
      res.json(user);
    });
    
    // POST create user
    app.post('/api/users', (req, res) => {
      const { name, email } = req.body;
      const newUser = {
        id: users.length + 1,
        name,
        email
      };
      users.push(newUser);
      res.status(201).json(newUser);
    });
    
    // PUT update user
    app.put('/api/users/:id', (req, res) => {
      const user = users.find(u => u.id === parseInt(req.params.id));
      if (!user) {
        return res.status(404).json({ error: 'User not found' });
      }
      user.name = req.body.name || user.name;
      user.email = req.body.email || user.email;
      res.json(user);
    });
    
    // DELETE user
    app.delete('/api/users/:id', (req, res) => {
      const index = users.findIndex(u => u.id === parseInt(req.params.id));
      if (index === -1) {
        return res.status(404).json({ error: 'User not found' });
      }
      users.splice(index, 1);
      res.status(204).send();
    });
    
    const PORT = process.env.PORT || 3000;
    app.listen(PORT, () => {
      console.log(`Server running on port ${PORT}`);
    });

---

## Asynchronous Programming in Node.js

Asynchronous programming is a fundamental aspect of Node.js, and understanding how to work with asynchronous code is crucial.

### Callbacks

    const fs = require('fs');
    
    // Callback pattern
    fs.readFile('file.txt', 'utf8', (err, data) => {
      if (err) {
        console.error('Error:', err);
        return;
      }
      console.log('Data:', data);
    });
    
    // Callback hell example
    fs.readFile('file1.txt', 'utf8', (err, data1) => {
      if (err) return console.error(err);
      fs.readFile('file2.txt', 'utf8', (err, data2) => {
        if (err) return console.error(err);
        fs.writeFile('output.txt', data1 + data2, (err) => {
          if (err) return console.error(err);
          console.log('Done');
        });
      });
    });

### Promises

    const fsPromises = require('fs').promises;
    
    // Using promises
    fsPromises.readFile('file.txt', 'utf8')
      .then(data => {
        console.log('Data:', data);
        return fsPromises.readFile('file2.txt', 'utf8');
      })
      .then(data2 => {
        console.log('Data2:', data2);
      })
      .catch(err => {
        console.error('Error:', err);
      });
    
    // Creating promises
    function readFilePromise(filename) {
      return new Promise((resolve, reject) => {
        fs.readFile(filename, 'utf8', (err, data) => {
          if (err) reject(err);
          else resolve(data);
        });
      });
    }
    
    // Promise.all - wait for all
    Promise.all([
      fsPromises.readFile('file1.txt', 'utf8'),
      fsPromises.readFile('file2.txt', 'utf8')
    ])
      .then(([data1, data2]) => {
        console.log(data1, data2);
      });
    
    // Promise.race - first to resolve
    Promise.race([
      fsPromises.readFile('file1.txt', 'utf8'),
      fsPromises.readFile('file2.txt', 'utf8')
    ])
      .then(data => {
        console.log('First file:', data);
      });

### Async/Await

    // Using async/await
    async function readFiles() {
      try {
        const data1 = await fsPromises.readFile('file1.txt', 'utf8');
        const data2 = await fsPromises.readFile('file2.txt', 'utf8');
        console.log(data1, data2);
      } catch (err) {
        console.error('Error:', err);
      }
    }
    
    // Multiple async operations
    async function readMultipleFiles() {
      try {
        const [data1, data2, data3] = await Promise.all([
          fsPromises.readFile('file1.txt', 'utf8'),
          fsPromises.readFile('file2.txt', 'utf8'),
          fsPromises.readFile('file3.txt', 'utf8')
        ]);
        return { data1, data2, data3 };
      } catch (err) {
        console.error('Error:', err);
      }
    }
    
    // Async function in Express route
    app.get('/api/data', async (req, res) => {
      try {
        const data = await fetchDataFromDatabase();
        res.json(data);
      } catch (err) {
        res.status(500).json({ error: err.message });
      }
    });

---

## Working with Databases in Node.js

Node.js supports various databases, both SQL and NoSQL.

### MongoDB with Mongoose

    // Installation
    // npm install mongoose
    
    const mongoose = require('mongoose');
    
    // Connect to MongoDB
    mongoose.connect('mongodb://localhost:27017/mydb', {
      useNewUrlParser: true,
      useUnifiedTopology: true
    });
    
    // Define schema
    const userSchema = new mongoose.Schema({
      name: { type: String, required: true },
      email: { type: String, required: true, unique: true },
      age: Number,
      createdAt: { type: Date, default: Date.now }
    });
    
    // Create model
    const User = mongoose.model('User', userSchema);
    
    // Create user
    const user = new User({
      name: 'John',
      email: 'john@example.com',
      age: 30
    });
    await user.save();
    
    // Find users
    const users = await User.find();
    const user = await User.findById(userId);
    const user = await User.findOne({ email: 'john@example.com' });
    
    // Update user
    await User.findByIdAndUpdate(userId, { age: 31 });
    
    // Delete user
    await User.findByIdAndDelete(userId);

### MySQL with mysql2

    // Installation
    // npm install mysql2
    
    const mysql = require('mysql2/promise');
    
    // Create connection
    const connection = await mysql.createConnection({
      host: 'localhost',
      user: 'root',
      password: 'password',
      database: 'mydb'
    });
    
    // Query
    const [rows] = await connection.execute(
      'SELECT * FROM users WHERE id = ?',
      [userId]
    );
    
    // Insert
    const [result] = await connection.execute(
      'INSERT INTO users (name, email) VALUES (?, ?)',
      ['John', 'john@example.com']
    );
    
    // Update
    await connection.execute(
      'UPDATE users SET name = ? WHERE id = ?',
      ['Jane', userId]
    );
    
    // Delete
    await connection.execute(
      'DELETE FROM users WHERE id = ?',
      [userId]
    );

### PostgreSQL with pg

    // Installation
    // npm install pg
    
    const { Pool } = require('pg');
    
    const pool = new Pool({
      user: 'postgres',
      host: 'localhost',
      database: 'mydb',
      password: 'password',
      port: 5432
    });
    
    // Query
    const result = await pool.query('SELECT * FROM users WHERE id = $1', [userId]);
    console.log(result.rows);

### ORMs

#### Sequelize (SQL)

    // Installation
    // npm install sequelize
    
    const { Sequelize, DataTypes } = require('sequelize');
    
    const sequelize = new Sequelize('database', 'username', 'password', {
      host: 'localhost',
      dialect: 'mysql'
    });
    
    // Define model
    const User = sequelize.define('User', {
      name: DataTypes.STRING,
      email: DataTypes.STRING
    });
    
    // CRUD operations
    await User.create({ name: 'John', email: 'john@example.com' });
    const users = await User.findAll();
    await User.update({ name: 'Jane' }, { where: { id: userId } });
    await User.destroy({ where: { id: userId } });

---

## Securing Node.js Applications

Security is a critical consideration in any application.

### Input Validation

    // Using express-validator
    // npm install express-validator
    
    const { body, validationResult } = require('express-validator');
    
    app.post('/api/users',
      body('email').isEmail(),
      body('password').isLength({ min: 8 }),
      (req, res) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) {
          return res.status(400).json({ errors: errors.array() });
        }
        // Process request
      }
    );

### Authentication and Authorization

    // Using JWT
    // npm install jsonwebtoken bcrypt
    
    const jwt = require('jsonwebtoken');
    const bcrypt = require('bcrypt');
    
    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Verify password
    const isValid = await bcrypt.compare(password, hashedPassword);
    
    // Generate token
    const token = jwt.sign({ userId: user.id }, 'secret-key', {
      expiresIn: '24h'
    });
    
    // Verify token middleware
    const authenticateToken = (req, res, next) => {
      const token = req.headers['authorization']?.split(' ')[1];
      if (!token) return res.sendStatus(401);
      
      jwt.verify(token, 'secret-key', (err, user) => {
        if (err) return res.sendStatus(403);
        req.user = user;
        next();
      });
    };

### Using HTTPS

    const https = require('https');
    const fs = require('fs');
    
    const options = {
      key: fs.readFileSync('key.pem'),
      cert: fs.readFileSync('cert.pem')
    };
    
    https.createServer(options, app).listen(443);

### Security Best Practices

1. **Use environment variables** for sensitive data
2. **Sanitize user input** to prevent injection attacks
3. **Use HTTPS** for all communications
4. **Implement rate limiting** to prevent abuse
5. **Use helmet** for security headers
6. **Keep dependencies updated**
7. **Use strong authentication** mechanisms
8. **Validate and sanitize** all inputs
9. **Use CORS** properly
10. **Implement logging** for security monitoring

---

## Testing and Debugging in Node.js

### Testing with Jest

    // Installation
    // npm install --save-dev jest
    
    // sum.js
    function sum(a, b) {
      return a + b;
    }
    module.exports = sum;
    
    // sum.test.js
    const sum = require('./sum');
    
    test('adds 1 + 2 to equal 3', () => {
      expect(sum(1, 2)).toBe(3);
    });
    
    // Run tests
    // npm test

### Testing with Mocha

    // Installation
    // npm install --save-dev mocha chai
    
    const assert = require('chai').assert;
    
    describe('Array', () => {
      describe('#indexOf()', () => {
        it('should return -1 when value is not present', () => {
          assert.equal([1, 2, 3].indexOf(4), -1);
        });
      });
    });

### Debugging

    // Using Node.js debugger
    // node --inspect index.js
    
    // Using console
    console.log('Debug message');
    console.error('Error message');
    console.table({ name: 'John', age: 30 });
    
    // Using debugger statement
    debugger;  // Pause execution if debugger is attached

---

## Scaling Node.js Applications

As your application grows, scaling becomes a consideration.

### Load Balancing

    // Using PM2 for process management
    // npm install -g pm2
    
    // Start application
    pm2 start index.js -i 4  // 4 instances
    
    // Load balancer with Nginx
    // nginx.conf
    upstream nodejs {
      server localhost:3000;
      server localhost:3001;
      server localhost:3002;
    }
    
    server {
      listen 80;
      location / {
        proxy_pass http://nodejs;
      }
    }

### Microservices Architecture

Break down a monolithic application into smaller, independent services that can be deployed and scaled individually.

    // Service 1: User Service
    // Service 2: Product Service
    // Service 3: Order Service
    // Each service can be deployed independently

### Caching

    // Using Redis
    // npm install redis
    
    const redis = require('redis');
    const client = redis.createClient();
    
    // Set cache
    await client.set('key', 'value', 'EX', 3600);  // Expire in 1 hour
    
    // Get cache
    const value = await client.get('key');
    
    // Using memory cache
    const NodeCache = require('node-cache');
    const cache = new NodeCache();
    
    cache.set('key', 'value', 3600);
    const value = cache.get('key');

### Environment Variables

    // Using dotenv
    // npm install dotenv
    
    require('dotenv').config();
    
    const PORT = process.env.PORT || 3000;
    const DB_URL = process.env.DATABASE_URL;
    
    // .env file
    PORT=3000
    DATABASE_URL=mongodb://localhost:27017/mydb
    JWT_SECRET=your-secret-key

---

## Best Practices

1. **Use async/await** instead of callbacks when possible
2. **Handle errors properly** with try/catch
3. **Use environment variables** for configuration
4. **Implement logging** (winston, pino)
5. **Use process managers** (PM2) for production
6. **Enable gzip compression**
7. **Use connection pooling** for databases
8. **Implement rate limiting**
9. **Use HTTPS** in production
10. **Keep dependencies updated**
11. **Write tests** for your code
12. **Use ESLint** for code quality
13. **Document your API** (Swagger/OpenAPI)
14. **Monitor your application** (New Relic, DataDog)

---

## Conclusion

Node.js is a powerful platform for building scalable server-side applications. By understanding its core concepts, asynchronous programming, and best practices, you can build robust and efficient applications. Continue learning and experimenting with different packages and patterns to become proficient in Node.js development.
