# Express.js Notes

## Table of Contents
1. [Introduction](#introduction)
2. [Installation & Setup](#installation--setup)
3. [Basic Application Structure](#basic-application-structure)
4. [Routing](#routing)
5. [Middleware](#middleware)
6. [Request & Response Objects](#request--response-objects)
7. [Template Engines](#template-engines)
8. [Error Handling](#error-handling)
9. [Static Files](#static-files)
10. [Best Practices](#best-practices)

---

## Introduction

**Express.js** is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications. It's built on top of Node.js and is one of the most popular web frameworks for Node.js.

### Key Features:
- Fast, unopinionated, minimalist web framework
- Robust routing system
- Middleware support
- Template engine integration
- Content negotiation
- Focus on high performance

---

## Installation & Setup

### Prerequisites
- Node.js installed on your system
- npm (Node Package Manager)

### Installation
```bash
# Initialize a new Node.js project
npm init -y

# Install Express.js
npm install express

# Install development dependencies (optional)
npm install --save-dev nodemon
```

### Basic Setup
```javascript
// app.js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
```

---

## Basic Application Structure

### Typical Project Structure
```
project-name/
│
├── node_modules/
├── public/
│   ├── css/
│   ├── js/
│   └── images/
├── routes/
│   ├── index.js
│   └── users.js
├── views/
│   └── index.ejs
├── middleware/
│   └── auth.js
├── models/
├── controllers/
├── app.js
├── package.json
└── package-lock.json
```

### Application Object
```javascript
const express = require('express');
const app = express();

// Application settings
app.set('view engine', 'ejs');
app.set('views', './views');

// Application-level middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
```

---

## Routing

### Basic Routing
```javascript
// GET route
app.get('/', (req, res) => {
  res.send('GET request to homepage');
});

// POST route
app.post('/users', (req, res) => {
  res.send('POST request to /users');
});

// PUT route
app.put('/users/:id', (req, res) => {
  res.send(`PUT request to /users/${req.params.id}`);
});

// DELETE route
app.delete('/users/:id', (req, res) => {
  res.send(`DELETE request to /users/${req.params.id}`);
});
```

### Route Parameters
```javascript
// Single parameter
app.get('/users/:id', (req, res) => {
  const userId = req.params.id;
  res.send(`User ID: ${userId}`);
});

// Multiple parameters
app.get('/users/:userId/posts/:postId', (req, res) => {
  const { userId, postId } = req.params;
  res.json({ userId, postId });
});

// Optional parameters
app.get('/posts/:year/:month?', (req, res) => {
  const { year, month } = req.params;
  res.json({ year, month: month || 'all' });
});
```

### Query Parameters
```javascript
app.get('/search', (req, res) => {
  const { q, page, limit } = req.query;
  res.json({
    query: q,
    page: page || 1,
    limit: limit || 10
  });
});
```

### Route Handlers
```javascript
// Multiple handlers
app.get('/protected', authenticate, authorize, (req, res) => {
  res.send('Protected content');
});

// Array of handlers
app.get('/multiple', [handler1, handler2, handler3]);

function authenticate(req, res, next) {
  // Authentication logic
  next();
}

function authorize(req, res, next) {
  // Authorization logic
  next();
}
```

### Express Router
```javascript
// routes/users.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.send('All users');
});

router.get('/:id', (req, res) => {
  res.send(`User ${req.params.id}`);
});

router.post('/', (req, res) => {
  res.send('Create user');
});

module.exports = router;

// app.js
const userRoutes = require('./routes/users');
app.use('/users', userRoutes);
```

---

## Middleware

### What is Middleware?
Middleware functions are functions that have access to the request object (`req`), response object (`res`), and the next middleware function in the application's request-response cycle.

### Types of Middleware

#### 1. Application-level Middleware
```javascript
// Executed for every request
app.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// Executed for specific path
app.use('/users', (req, res, next) => {
  console.log('Request to /users');
  next();
});
```

#### 2. Router-level Middleware
```javascript
const router = express.Router();

router.use((req, res, next) => {
  console.log('Router middleware');
  next();
});

router.get('/users', (req, res) => {
  res.send('Users page');
});
```

#### 3. Error-handling Middleware
```javascript
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

#### 4. Built-in Middleware
```javascript
// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));
```

#### 5. Third-party Middleware
```javascript
const cors = require('cors');
const morgan = require('morgan');
const helmet = require('helmet');

app.use(cors());
app.use(morgan('combined'));
app.use(helmet());
```

### Custom Middleware Examples
```javascript
// Logging middleware
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
  next();
};

// Authentication middleware
const requireAuth = (req, res, next) => {
  const token = req.headers.authorization;
  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }
  // Verify token logic here
  next();
};

// Rate limiting middleware
const rateLimit = (windowMs, maxRequests) => {
  const requests = new Map();
  
  return (req, res, next) => {
    const ip = req.ip;
    const now = Date.now();
    const windowStart = now - windowMs;
    
    if (!requests.has(ip)) {
      requests.set(ip, []);
    }
    
    const userRequests = requests.get(ip);
    const recentRequests = userRequests.filter(time => time > windowStart);
    
    if (recentRequests.length >= maxRequests) {
      return res.status(429).json({ error: 'Too many requests' });
    }
    
    recentRequests.push(now);
    requests.set(ip, recentRequests);
    next();
  };
};

app.use(logger);
app.use('/api', rateLimit(60000, 100)); // 100 requests per minute
```

---

## Request & Response Objects

### Request Object (req)
```javascript
app.get('/example', (req, res) => {
  // Request properties
  console.log(req.params);    // Route parameters
  console.log(req.query);     // Query string parameters
  console.log(req.body);      // Request body (with body parser)
  console.log(req.headers);   // Request headers
  console.log(req.method);    // HTTP method
  console.log(req.url);       // Request URL
  console.log(req.path);      // Request path
  console.log(req.protocol);  // Protocol (http/https)
  console.log(req.ip);        // Client IP address
  console.log(req.cookies);   // Cookies (with cookie parser)
});
```

### Response Object (res)
```javascript
app.get('/response-example', (req, res) => {
  // Send different types of responses
  
  // Send text
  res.send('Hello World');
  
  // Send JSON
  res.json({ message: 'Hello', status: 'success' });
  
  // Send with status code
  res.status(201).json({ message: 'Created' });
  
  // Set headers
  res.set('X-Custom-Header', 'value');
  res.header('Content-Type', 'application/json');
  
  // Redirect
  res.redirect('/login');
  res.redirect(301, '/new-url');
  
  // Send file
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
  
  // Download file
  res.download('/path/to/file.pdf');
  
  // Set cookies
  res.cookie('name', 'value', { maxAge: 900000, httpOnly: true });
  
  // Clear cookies
  res.clearCookie('name');
  
  // End response
  res.end();
});
```

---

## Template Engines

### Setting up EJS
```bash
npm install ejs
```

```javascript
// app.js
app.set('view engine', 'ejs');
app.set('views', './views');

app.get('/', (req, res) => {
  const data = {
    title: 'Home Page',
    users: ['John', 'Jane', 'Bob']
  };
  res.render('index', data);
});
```

```html
<!-- views/index.ejs -->
<!DOCTYPE html>
<html>
<head>
    <title><%= title %></title>
</head>
<body>
    <h1><%= title %></h1>
    <ul>
        <% users.forEach(user => { %>
            <li><%= user %></li>
        <% }); %>
    </ul>
</body>
</html>
```

### Setting up Handlebars
```bash
npm install express-handlebars
```

```javascript
const exphbs = require('express-handlebars');

app.engine('handlebars', exphbs());
app.set('view engine', 'handlebars');

app.get('/', (req, res) => {
  res.render('home', {
    title: 'Home Page',
    message: 'Welcome to Express!'
  });
});
```

---

## Error Handling

### Basic Error Handling
```javascript
// Synchronous error handling
app.get('/sync-error', (req, res, next) => {
  try {
    // Some operation that might throw an error
    const result = JSON.parse('invalid json');
    res.json(result);
  } catch (error) {
    next(error);
  }
});

// Asynchronous error handling
app.get('/async-error', async (req, res, next) => {
  try {
    const data = await someAsyncOperation();
    res.json(data);
  } catch (error) {
    next(error);
  }
});
```

### Error Handling Middleware
```javascript
// 404 handler (should be last route)
app.use('*', (req, res) => {
  res.status(404).json({
    error: 'Not Found',
    message: `Route ${req.originalUrl} not found`
  });
});

// Global error handler (should be last middleware)
app.use((err, req, res, next) => {
  console.error(err.stack);
  
  // Default error
  let error = {
    message: err.message || 'Internal Server Error',
    status: err.status || 500
  };
  
  // Development error details
  if (process.env.NODE_ENV === 'development') {
    error.stack = err.stack;
  }
  
  res.status(error.status).json(error);
});
```

### Custom Error Classes
```javascript
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.status = `${statusCode}`.startsWith('4') ? 'fail' : 'error';
    this.isOperational = true;
    
    Error.captureStackTrace(this, this.constructor);
  }
}

// Usage
app.get('/protected', (req, res, next) => {
  if (!req.headers.authorization) {
    return next(new AppError('No authorization header', 401));
  }
  res.json({ message: 'Protected content' });
});
```

---

## Static Files

### Serving Static Files
```javascript
// Serve static files from 'public' directory
app.use(express.static('public'));

// Serve with virtual path prefix
app.use('/static', express.static('public'));

// Multiple static directories
app.use(express.static('public'));
app.use(express.static('files'));

// Static files with options
app.use('/static', express.static('public', {
  dotfiles: 'ignore',
  etag: false,
  extensions: ['htm', 'html'],
  index: false,
  maxAge: '1d',
  redirect: false,
  setHeaders: function (res, path, stat) {
    res.set('x-timestamp', Date.now());
  }
}));
```

---

## Best Practices

### 1. Environment Configuration
```javascript
// Use environment variables
const PORT = process.env.PORT || 3000;
const DB_URL = process.env.DATABASE_URL || 'mongodb://localhost/myapp';

// Use dotenv for development
require('dotenv').config();
```

### 2. Security Best Practices
```javascript
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

// Use Helmet for security headers
app.use(helmet());

// Rate limiting
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});
app.use(limiter);

// Input validation
const { body, validationResult } = require('express-validator');

app.post('/users',
  body('email').isEmail(),
  body('password').isLength({ min: 6 }),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Process valid input
  }
);
```

### 3. Code Organization
```javascript
// controllers/userController.js
exports.getUsers = async (req, res, next) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    next(error);
  }
};

// routes/userRoutes.js
const express = require('express');
const userController = require('../controllers/userController');
const router = express.Router();

router.get('/', userController.getUsers);

module.exports = router;

// app.js
const userRoutes = require('./routes/userRoutes');
app.use('/api/users', userRoutes);
```

### 4. Graceful Shutdown
```javascript
const server = app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

// Graceful shutdown
process.on('SIGTERM', () => {
  console.log('SIGTERM received, shutting down gracefully');
  server.close(() => {
    console.log('Process terminated');
  });
});

process.on('SIGINT', () => {
  console.log('SIGINT received, shutting down gracefully');
  server.close(() => {
    console.log('Process terminated');
  });
});
```

### 5. Logging
```javascript
const morgan = require('morgan');

// HTTP request logging
app.use(morgan('combined'));

// Custom logging
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}
```

---

## Common Express.js Patterns

### RESTful API Structure
```javascript
// GET /api/users - Get all users
// GET /api/users/:id - Get a specific user
// POST /api/users - Create a new user
// PUT /api/users/:id - Update a user
// DELETE /api/users/:id - Delete a user

const router = express.Router();

router.route('/users')
  .get(getUsers)
  .post(createUser);

router.route('/users/:id')
  .get(getUser)
  .put(updateUser)
  .delete(deleteUser);
```

### Async/Await Wrapper
```javascript
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

---

## Useful Express.js Packages

- **cors**: Enable CORS
- **helmet**: Security middleware
- **morgan**: HTTP request logger
- **express-validator**: Input validation
- **express-rate-limit**: Rate limiting
- **compression**: Gzip compression
- **cookie-parser**: Parse cookies
- **express-session**: Session management
- **passport**: Authentication
- **multer**: File upload handling
- **express-handlebars**: Handlebars template engine

---

This comprehensive guide covers the essential concepts and features of Express.js. Practice building applications with these concepts to master Express.js development!