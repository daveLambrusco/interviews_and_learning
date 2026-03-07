# Node.js Backend Developer Interview Q&A

- [Node.js Backend Developer Interview Q\&A](#nodejs-backend-developer-interview-qa)
  - [Core Node.js Concepts](#core-nodejs-concepts)
    - [Q1: What is Node.js and what makes it different from traditional server-side technologies?](#q1-what-is-nodejs-and-what-makes-it-different-from-traditional-server-side-technologies)
    - [Q2: Explain the Event Loop in Node.js](#q2-explain-the-event-loop-in-nodejs)
    - [Q3: What's the difference between process.nextTick(), setImmediate(), and setTimeout()?](#q3-whats-the-difference-between-processnexttick-setimmediate-and-settimeout)
    - [Q4: What are Streams in Node.js and when would you use them?](#q4-what-are-streams-in-nodejs-and-when-would-you-use-them)
    - [Q5: Explain callback hell and how to avoid it](#q5-explain-callback-hell-and-how-to-avoid-it)
  - [REST API Development](#rest-api-development)
    - [Q6: How would you design and structure a RESTful API in Node.js?](#q6-how-would-you-design-and-structure-a-restful-api-in-nodejs)
    - [Q7: What's the difference between PUT and PATCH?](#q7-whats-the-difference-between-put-and-patch)
    - [Q8: How do you handle authentication and authorization in Node.js REST APIs?](#q8-how-do-you-handle-authentication-and-authorization-in-nodejs-rest-apis)
    - [Q9: How do you handle errors in Node.js REST APIs?](#q9-how-do-you-handle-errors-in-nodejs-rest-apis)
    - [Q10: How would you implement pagination, filtering, and sorting in a REST API?](#q10-how-would-you-implement-pagination-filtering-and-sorting-in-a-rest-api)
  - [Express.js \& MVC Frameworks](#expressjs--mvc-frameworks)
    - [Q11: Explain the middleware pattern in Express.js](#q11-explain-the-middleware-pattern-in-expressjs)
    - [Q12: What is the MVC pattern and how do you implement it in Node.js?](#q12-what-is-the-mvc-pattern-and-how-do-you-implement-it-in-nodejs)
    - [Q13: What popular Node.js frameworks are you familiar with?](#q13-what-popular-nodejs-frameworks-are-you-familiar-with)
  - [Database Integration](#database-integration)
    - [Q14: How do you work with databases in Node.js? Compare SQL vs NoSQL approaches](#q14-how-do-you-work-with-databases-in-nodejs-compare-sql-vs-nosql-approaches)
    - [Q15: How do you handle database transactions in Node.js?](#q15-how-do-you-handle-database-transactions-in-nodejs)
    - [Q16: How do you optimize database queries in Node.js?](#q16-how-do-you-optimize-database-queries-in-nodejs)
  - [Async Programming](#async-programming)
    - [Q17: Explain Promises vs Async/Await](#q17-explain-promises-vs-asyncawait)
    - [Q18: How do you handle multiple async operations?](#q18-how-do-you-handle-multiple-async-operations)
  - [Performance \& Security](#performance--security)
    - [Q19: How do you optimize Node.js application performance?](#q19-how-do-you-optimize-nodejs-application-performance)
    - [Q20: What security best practices do you follow in Node.js?](#q20-what-security-best-practices-do-you-follow-in-nodejs)
  - [Docker \& DevOps](#docker--devops)
    - [Q21: How do you containerize a Node.js application with Docker?](#q21-how-do-you-containerize-a-nodejs-application-with-docker)
    - [Q22: What's your experience with AWS services for Node.js applications?](#q22-whats-your-experience-with-aws-services-for-nodejs-applications)
    - [Q23: How do you implement CI/CD for Node.js applications?](#q23-how-do-you-implement-cicd-for-nodejs-applications)
  - [Advanced Topics](#advanced-topics)
    - [Q24: How do you implement WebSockets in Node.js?](#q24-how-do-you-implement-websockets-in-nodejs)
    - [Q25: How do you handle file uploads in Node.js?](#q25-how-do-you-handle-file-uploads-in-nodejs)
    - [Q26: How do you implement background jobs and task queues?](#q26-how-do-you-implement-background-jobs-and-task-queues)
    - [Q27: How do you implement testing in Node.js?](#q27-how-do-you-implement-testing-in-nodejs)
    - [Q28: How do you debug Node.js applications?](#q28-how-do-you-debug-nodejs-applications)
    - [Q29: Describe a challenging Node.js project you worked on](#q29-describe-a-challenging-nodejs-project-you-worked-on)
    - [Q30: How do you stay updated with Node.js and JavaScript ecosystem?](#q30-how-do-you-stay-updated-with-nodejs-and-javascript-ecosystem)
  - [Additional Core Concepts \& Fundamentals](#additional-core-concepts--fundamentals)
    - [Q31: What is REPL in Node.js?](#q31-what-is-repl-in-nodejs)
    - [Q32: What is the Event Emitter pattern?](#q32-what-is-the-event-emitter-pattern)
    - [Q33: What's the difference between `process.nextTick()` and `setImmediate()`?](#q33-whats-the-difference-between-processnexttick-and-setimmediate)
    - [Q34: How does Node.js handle child processes?](#q34-how-does-nodejs-handle-child-processes)
    - [Q35: What is the difference between operational and programmer errors?](#q35-what-is-the-difference-between-operational-and-programmer-errors)
    - [Q36: What is the purpose of `module.exports` vs `exports`?](#q36-what-is-the-purpose-of-moduleexports-vs-exports)
    - [Q37: What is the buffer class in Node.js?](#q37-what-is-the-buffer-class-in-nodejs)
    - [Q38: What are the different types of HTTP requests in Node.js?](#q38-what-are-the-different-types-of-http-requests-in-nodejs)
    - [Q39: How does Node.js support multi-core processors?](#q39-how-does-nodejs-support-multi-core-processors)
    - [Q40: What is middleware in Node.js/Express?](#q40-what-is-middleware-in-nodejsexpress)
  - [Advanced Topics \& Modern Practices](#advanced-topics--modern-practices)
    - [Q41: Explain package.json and important fields](#q41-explain-packagejson-and-important-fields)
    - [Q42: How do you implement TypeScript with Node.js?](#q42-how-do-you-implement-typescript-with-nodejs)
    - [Q43: How do you implement logging in Node.js?](#q43-how-do-you-implement-logging-in-nodejs)
    - [Q44: What are Worker Threads and when should you use them?](#q44-what-are-worker-threads-and-when-should-you-use-them)
    - [Q45: How do you implement caching strategies in Node.js?](#q45-how-do-you-implement-caching-strategies-in-nodejs)
    - [Q46: How do you handle file uploads securely?](#q46-how-do-you-handle-file-uploads-securely)
    - [Q47: How do you implement rate limiting in Node.js?](#q47-how-do-you-implement-rate-limiting-in-nodejs)
    - [Q48: How do you implement real-time features with Socket.IO?](#q48-how-do-you-implement-real-time-features-with-socketio)
    - [Q49: How do you implement testing strategies in Node.js?](#q49-how-do-you-implement-testing-strategies-in-nodejs)
    - [Q50: What deployment strategies do you use for Node.js applications?](#q50-what-deployment-strategies-do-you-use-for-nodejs-applications)

## Core Node.js Concepts

### Q1: What is Node.js and what makes it different from traditional server-side technologies?

**A:** Node.js is a JavaScript runtime built on Chrome's V8 engine that allows you to run JavaScript on the server side. Key differences:

- **Event-driven, non-blocking I/O**: Unlike traditional threaded models (PHP, Java), Node.js uses a single-threaded event loop to handle concurrent requests efficiently
- **Asynchronous by default**: Operations like file I/O, database queries, and HTTP requests don't block the main thread
- **Same language for frontend and backend**: Enables code sharing and easier full-stack development
- **NPM ecosystem**: Access to the largest package registry in the world

### Q2: Explain the Event Loop in Node.js

**A:** The Event Loop is the core mechanism that makes Node.js non-blocking:

1. **Call Stack**: Executes synchronous code
2. **Node APIs**: Handles async operations (timers, I/O, etc.)
3. **Callback Queue**: Stores callbacks from completed async operations
4. **Event Loop**: Continuously checks if the call stack is empty, then pushes callbacks from the queue to the stack

Phases of the Event Loop:

- **Timers**: Executes setTimeout/setInterval callbacks
- **Pending callbacks**: I/O callbacks deferred to next loop
- **Idle, prepare**: Internal use
- **Poll**: Retrieves new I/O events
- **Check**: setImmediate callbacks
- **Close callbacks**: socket.on('close', ...)

### Q3: What's the difference between process.nextTick(), setImmediate(), and setTimeout()?

**A:** These are all ways to schedule asynchronous code execution, but they differ in **when** they run in the event loop:

**process.nextTick():**

- Executes **immediately** after the current operation completes
- Runs **before** any I/O events or timers
- Has the **highest priority** (runs before even Promise callbacks in older Node versions)
- Can **starve** the event loop if called recursively (blocks I/O)

**setImmediate():**

- Executes in the **check phase** of the event loop
- Runs **after** I/O events have been processed
- Better for recursive operations (doesn't block I/O)

**setTimeout(fn, 0):**

- Executes in the **timers phase** of the event loop
- Has the **lowest priority** among these three
- Minimum delay is ~1ms (not truly 0ms)

**Execution Order Visualization:**

```
Current Operation Completes
         ↓
   process.nextTick() ←── Runs FIRST (highest priority)
         ↓
   Promise callbacks (microtasks)
         ↓
   Event Loop Phases:
     → Timers (setTimeout/setInterval)
     → I/O callbacks
     → Poll (retrieve I/O events)
     → Check (setImmediate) ←── Runs HERE
     → Close callbacks
```

**Practical Example:**

```javascript
console.log('1: Script start');

setTimeout(() => console.log('2: setTimeout'), 0);

setImmediate(() => console.log('3: setImmediate'));

process.nextTick(() => console.log('4: nextTick'));

Promise.resolve().then(() => console.log('5: Promise'));

console.log('6: Script end');

// Output:
// 1: Script start
// 6: Script end
// 4: nextTick          ← Runs immediately after synchronous code
// 5: Promise           ← Microtask queue
// 2: setTimeout        ← Timers phase (or might swap with setImmediate)
// 3: setImmediate      ← Check phase (or might swap with setTimeout)
```

**Important Note:** The order of setTimeout vs setImmediate can vary depending on the context. Inside an I/O callback, setImmediate will **always** run first.

**When to use each:**

- **process.nextTick()**: When you need something to run before any I/O (use sparingly!)
- **setImmediate()**: For breaking up CPU-intensive tasks (recommended)
- **setTimeout()**: For actual delays or scheduling

**See also:** [Q33](#q33-whats-the-difference-between-processnexttick-and-setimmediate) for deeper dive, [Q2](#q2-explain-the-event-loop-in-nodejs) for Event Loop phases

### Q4: What are Streams in Node.js and when would you use them?

**A:** Streams are objects that let you read/write data in chunks rather than all at once, making them memory-efficient for large data.

Types:

- **Readable**: Reading data (fs.createReadStream, HTTP request)
- **Writable**: Writing data (fs.createWriteStream, HTTP response)
- **Duplex**: Both readable and writable (TCP socket)
- **Transform**: Modify data while reading/writing (zlib, crypto)

Use cases:

- Processing large files without loading entire file into memory
- Streaming video/audio content
- Data transformation pipelines
- Real-time data processing

```javascript
const fs = require('fs');
const readStream = fs.createReadStream('large-file.txt');
const writeStream = fs.createWriteStream('output.txt');
readStream.pipe(writeStream);
```

### Q5: Explain callback hell and how to avoid it

**A:** Callback hell occurs when multiple nested callbacks make code hard to read and maintain:

```javascript
// Bad - Callback hell
getData(function(a) {
  getMoreData(a, function(b) {
    getMoreData(b, function(c) {
      getMoreData(c, function(d) {
        // ...
      });
    });
  });
});
```

Solutions:

1. **Promises**: Chain .then() calls
2. **Async/Await**: Write asynchronous code that looks synchronous
3. **Modularization**: Break code into smaller, named functions
4. **Control flow libraries**: async.js, but now mostly replaced by native async/await

```javascript
// Good - Async/Await
async function processData() {
  const a = await getData();
  const b = await getMoreData(a);
  const c = await getMoreData(b);
  const d = await getMoreData(c);
  return d;
}
```

## REST API Development

### Q6: How would you design and structure a RESTful API in Node.js?

Best practices for REST API structure:

**1. Project Structure:**

```
src/
├── controllers/     # Handle requests, responses
├── models/         # Data models (Mongoose, Sequelize)
├── routes/         # API endpoints
├── middleware/     # Auth, validation, error handling
├── services/       # Business logic
├── utils/          # Helper functions
├── config/         # Configuration files
└── app.js          # Application entry
```

**2. RESTful Principles:**

- Use HTTP methods correctly: GET (read), POST (create), PUT/PATCH (update), DELETE (delete)
- Use proper HTTP status codes (200, 201, 400, 401, 404, 500, etc.)
- Use plural nouns for resources: `/api/users`, not `/api/user`
- Use nested routes for relationships: `/api/users/:id/posts`
- Version your API: `/api/v1/users`

**3. Example Implementation:**

```javascript
// routes/users.js
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController');
const auth = require('../middleware/auth');

router.get('/', auth, userController.getAllUsers);
router.get('/:id', auth, userController.getUserById);
router.post('/', auth, userController.createUser);
router.put('/:id', auth, userController.updateUser);
router.delete('/:id', auth, userController.deleteUser);

module.exports = router;
```

### Q7: What's the difference between PUT and PATCH?

- **PUT**: Replaces the entire resource. Client must send all fields.
- **PATCH**: Partially updates the resource. Client sends only changed fields.

Example:

```javascript
// PUT /api/users/1
{ "name": "John", "email": "john@example.com", "age": 30 }
// Must include all fields

// PATCH /api/users/1
{ "email": "newemail@example.com" }
// Only the fields to update
```

### Q8: How do you handle authentication and authorization in Node.js REST APIs?

**A:** Authentication verifies **who you are** (identity), while authorization determines **what you can access** (permissions).

**Understanding the Flow:**

1. User sends credentials (username/password)
2. Server verifies credentials
3. Server generates and returns a token (JWT) or session
4. Client stores token (localStorage, cookie)
5. Client sends token with each subsequent request
6. Server validates token and grants/denies access

**Common Approaches:**

**1. JWT (JSON Web Tokens) - Most Popular for REST APIs:**

```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcrypt');

// STEP 1: Login endpoint - Generate token
app.post('/api/auth/login', async (req, res) => {
  const { email, password } = req.body;
  
  // Find user in database
  const user = await User.findOne({ email });
  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Verify password
  const isValidPassword = await bcrypt.compare(password, user.password);
  if (!isValidPassword) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  
  // Generate JWT token (contains user info + signature)
  const token = jwt.sign(
    { 
      userId: user.id, 
      email: user.email,
      role: user.role  // For authorization
    },
    process.env.JWT_SECRET,  // Secret key (never expose!)
    { expiresIn: '24h' }     // Token expires after 24 hours
  );
  
  res.json({ 
    token,
    user: { id: user.id, email: user.email, name: user.name }
  });
});

// STEP 2: Authentication Middleware - Verify token
const authMiddleware = async (req, res, next) => {
  try {
    // Extract token from Authorization header
    // Format: "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6..."
    const token = req.header('Authorization')?.replace('Bearer ', '');
    
    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }
    
    // Verify token signature and expiration
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // Attach user info to request object
    req.user = decoded;
    
    // Optionally: fetch full user from database
    // req.user = await User.findById(decoded.userId);
    
    next(); // Continue to route handler
  } catch (error) {
    if (error.name === 'TokenExpiredError') {
      return res.status(401).json({ error: 'Token expired' });
    }
    res.status(401).json({ error: 'Invalid token' });
  }
};

// STEP 3: Use middleware to protect routes
app.get('/api/profile', authMiddleware, async (req, res) => {
  // req.user is available here (from middleware)
  const user = await User.findById(req.user.userId);
  res.json(user);
});
```

**2. Session-based Authentication:**

```javascript
const session = require('express-session');
const MongoStore = require('connect-mongo');

app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  store: MongoStore.create({ mongoUrl: process.env.MONGODB_URI }),
  cookie: {
    maxAge: 1000 * 60 * 60 * 24, // 24 hours
    httpOnly: true,  // Prevents XSS attacks
    secure: process.env.NODE_ENV === 'production' // HTTPS only in production
  }
}));

app.post('/api/auth/login', async (req, res) => {
  const user = await User.findOne({ email: req.body.email });
  // ...verify password...
  
  req.session.userId = user.id; // Store in session
  res.json({ message: 'Logged in' });
});

const authMiddleware = (req, res, next) => {
  if (!req.session.userId) {
    return res.status(401).json({ error: 'Not authenticated' });
  }
  next();
};
```

**3. OAuth 2.0** (Google, GitHub, etc.):

```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "/auth/google/callback"
  },
  async (accessToken, refreshToken, profile, done) => {
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
  }
));
```

**Authorization (Role-Based Access Control):**

```javascript
// Middleware to check user roles
const authorize = (...allowedRoles) => {
  return (req, res, next) => {
    // req.user comes from authMiddleware
    if (!req.user) {
      return res.status(401).json({ error: 'Not authenticated' });
    }
    
    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden: Insufficient permissions' });
    }
    
    next();
  };
};

// Usage: Chain authentication + authorization
router.get('/admin/users', 
  authMiddleware,              // First: verify token
  authorize('admin'),          // Then: check if user is admin
  getAllUsers
);

router.delete('/users/:id', 
  authMiddleware, 
  authorize('admin', 'moderator'),  // Multiple roles allowed
  deleteUser
);

router.get('/profile', 
  authMiddleware,              // No authorization - any authenticated user
  getProfile
);
```

**Security Best Practices (2026):**

- Store JWT secret in environment variables
- Use HTTPS in production
- Set short token expiration (1-24 hours)
- Implement refresh tokens for long sessions
- Hash passwords with bcrypt (10+ rounds)
- Use httpOnly cookies to prevent XSS
- Implement rate limiting on login endpoints
- Add CSRF protection for session-based auth
- Validate and sanitize all inputs
- Log authentication attempts

**See also:** [Q20 for more security practices](#q20-what-security-best-practices-do-you-follow-in-nodejs)

### Q9: How do you handle errors in Node.js REST APIs?

**A:** Error handling in Express requires understanding how errors flow through the middleware chain and how to catch async errors properly.

**The Problem with Async Errors:**

```javascript
// ❌ WRONG - Async errors are NOT caught by Express by default
app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id); // If this throws, app crashes!
  res.json(user);
});

// ✅ CORRECT - Must use try/catch
app.get('/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    res.json(user);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

**Centralized Error Handling Solution:**

**Step 1: Create Custom Error Class**

```javascript
// Custom error class for operational errors
class AppError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.statusCode = statusCode;
    this.isOperational = true; // Distinguishes operational vs programmer errors
    Error.captureStackTrace(this, this.constructor);
  }
}

// Usage
throw new AppError('User not found', 404);
throw new AppError('Invalid input', 400);
```

**Step 2: Create asyncHandler Wrapper**

The `asyncHandler` is a **higher-order function** that wraps async route handlers and automatically catches errors:

```javascript
// How asyncHandler works:
const asyncHandler = (fn) => {
  // Returns a new function that Express will call
  return (req, res, next) => {
    // Execute the async function and catch any errors
    Promise.resolve(fn(req, res, next))
      .catch(next); // Pass error to next() -> goes to error middleware
  };
};

// Detailed explanation:
// 1. asyncHandler takes a function (fn) as parameter
// 2. Returns a new function with (req, res, next) - Express middleware signature
// 3. Calls fn(req, res, next) - your async route handler
// 4. Wraps it in Promise.resolve() - handles both promises and regular functions
// 5. .catch(next) - if fn throws or rejects, passes error to next()
// 6. next(error) triggers Express error handling middleware
```

**How Errors Flow Through Express:**

```
Route Handler throws error
         ↓
   asyncHandler catches it
         ↓
   calls next(error)
         ↓
   Express skips normal middleware
         ↓
   Goes directly to Error Handling Middleware (4 parameters)
         ↓
   Error middleware processes error
         ↓
   Sends response to client
```

**Step 3: Create Error Handling Middleware**

```javascript
// Error handling middleware - MUST have 4 parameters!
// Must be defined AFTER all routes
const errorHandler = (err, req, res, next) => {
  // Set defaults
  err.statusCode = err.statusCode || 500;
  err.message = err.message || 'Internal Server Error';
  
  // Log error (use winston/pino in production)
  console.error('Error:', {
    message: err.message,
    stack: err.stack,
    url: req.url,
    method: req.method
  });
  
  // Development: return full error details
  if (process.env.NODE_ENV === 'development') {
    return res.status(err.statusCode).json({
      error: {
        message: err.message,
        stack: err.stack,
        statusCode: err.statusCode
      }
    });
  }
  
  // Production: hide internal errors
  res.status(err.statusCode).json({
    error: {
      message: err.isOperational ? err.message : 'Something went wrong'
    }
  });
};
```

**Step 4: Complete Implementation**

```javascript
const express = require('express');
const app = express();

// Normal middleware
app.use(express.json());

// Routes with asyncHandler
app.get('/users/:id', asyncHandler(async (req, res, next) => {
  const user = await User.findById(req.params.id);
  
  if (!user) {
    // Throw custom error - asyncHandler catches it and calls next(error)
    throw new AppError('User not found', 404);
  }
  
  res.json(user);
}));

app.post('/users', asyncHandler(async (req, res, next) => {
  const { email, name } = req.body;
  
  // Validation error
  if (!email || !name) {
    throw new AppError('Email and name are required', 400);
  }
  
  // Check if user exists
  const existingUser = await User.findOne({ email });
  if (existingUser) {
    throw new AppError('Email already in use', 409);
  }
  
  const user = await User.create({ email, name });
  res.status(201).json(user);
}));

// Catch 404 - No route matched
app.use((req, res, next) => {
  next(new AppError(`Route ${req.url} not found`, 404));
});

// Error handling middleware - MUST be last!
app.use(errorHandler);

// Handle uncaught errors outside Express
process.on('unhandledRejection', (err) => {
  console.error('UNHANDLED REJECTION:', err);
  process.exit(1);
});

app.listen(3000);
```

**Alternative: Express 5 (Native Async Support)**

```javascript
// Express 5+ catches async errors automatically - no asyncHandler needed!
app.get('/users/:id', async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    throw new AppError('User not found', 404); // Automatically caught
  }
  res.json(user);
});
```

**Different Types of Errors:**

```javascript
// 1. Validation Errors (400)
throw new AppError('Invalid email format', 400);

// 2. Authentication Errors (401)
throw new AppError('Invalid credentials', 401);

// 3. Authorization Errors (403)
throw new AppError('You don't have permission', 403);

// 4. Not Found Errors (404)
throw new AppError('User not found', 404);

// 5. Conflict Errors (409)
throw new AppError('Email already exists', 409);

// 6. Server Errors (500)
throw new AppError('Database connection failed', 500);
```

**Best Practices:**

- Use custom error classes for different error types
- Always use asyncHandler or try/catch with async routes
- Log all errors (use [Winston/Pino - see Q43](#q43-how-do-you-implement-logging-in-nodejs))
- Never expose internal errors in production
- Return appropriate HTTP status codes
- Include request context in error logs
- Use error monitoring services (Sentry, Rollbar)

**See also:** [Q35 for operational vs programmer errors](#q35-what-is-the-difference-between-operational-and-programmer-errors)

### Q10: How would you implement pagination, filtering, and sorting in a REST API?

**A:** Understanding the User Model and Mongoose Query Methods

**What is the User object?**
`User` is a **Mongoose Model** - a constructor function that provides an interface to interact with MongoDB collections:

```javascript
// Step 1: Define Schema (structure of documents)
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  role: { type: String, enum: ['user', 'admin', 'moderator'], default: 'user' },
  status: { type: String, enum: ['active', 'inactive'], default: 'active' },
  createdAt: { type: Date, default: Date.now }
});

// Step 2: Create Model from Schema
const User = mongoose.model('User', userSchema);
// User is now a class with built-in methods for database operations
```

**Mongoose Query Methods Explained:**

- **`.find(filter)`** - Returns multiple documents matching filter
- **`.findOne(filter)`** - Returns single document
- **`.findById(id)`** - Returns document by ID
- **`.sort(sortObject)`** - Sorts results (1 = ascending, -1 = descending)
- **`.skip(number)`** - Skips first N documents (for pagination)
- **`.limit(number)`** - Limits number of documents returned
- **`.countDocuments(filter)`** - Counts documents matching filter

These methods are **chainable** and build a query that executes when `await` is called.

**Complete Implementation with Explanations:**

```javascript
const getAllUsers = async (req, res) => {
  // PAGINATION
  // Extract page and limit from query parameters
  // Example: /api/users?page=2&limit=20
  const page = parseInt(req.query.page) || 1;      // Default: page 1
  const limit = parseInt(req.query.limit) || 10;   // Default: 10 items per page
  const skip = (page - 1) * limit;                 // Calculate items to skip
  
  // Example: page=3, limit=10 → skip = (3-1)*10 = 20 (skip first 20 items)
  
  // FILTERING
  // Build filter object dynamically from query parameters
  // Example: /api/users?role=admin&status=active
  const filter = {};
  
  if (req.query.role) {
    filter.role = req.query.role;  // filter = { role: 'admin' }
  }
  
  if (req.query.status) {
    filter.status = req.query.status;  // filter = { role: 'admin', status: 'active' }
  }
  
  // Advanced filtering examples:
  if (req.query.search) {
    // Text search (case-insensitive)
    filter.name = { $regex: req.query.search, $options: 'i' };
  }
  
  if (req.query.minAge) {
    filter.age = { $gte: parseInt(req.query.minAge) };  // Greater than or equal
  }
  
  // SORTING
  // Build sort object from query parameter
  // Example: /api/users?sortBy=createdAt:desc
  const sort = {};
  
  if (req.query.sortBy) {
    const parts = req.query.sortBy.split(':');
    const field = parts[0];           // 'createdAt'
    const order = parts[1];           // 'desc'
    sort[field] = order === 'desc' ? -1 : 1;  // -1 for descending, 1 for ascending
  }
  
  // EXECUTE QUERY
  // Chain Mongoose methods to build and execute query
  const users = await User
    .find(filter)          // Find documents matching filter
    .sort(sort)            // Sort results
    .skip(skip)            // Skip first N documents
    .limit(limit)          // Limit to N documents
    .select('-password');  // Exclude password field
  
  // Get total count for pagination metadata
  const total = await User.countDocuments(filter);
  
  // Calculate pagination metadata
  const totalPages = Math.ceil(total / limit);
  const hasNextPage = page < totalPages;
  const hasPrevPage = page > 1;
  
  // Return paginated response
  res.json({
    success: true,
    data: users,
    pagination: {
      page,
      limit,
      total,              // Total documents matching filter
      totalPages,
      hasNextPage,
      hasPrevPage,
      nextPage: hasNextPage ? page + 1 : null,
      prevPage: hasPrevPage ? page - 1 : null
    }
  });
};

// Example API calls and their queries:

// 1. GET /api/users?page=1&limit=10
//    → First 10 users

// 2. GET /api/users?page=2&limit=20&role=admin
//    → Users 21-40 with role='admin'

// 3. GET /api/users?sortBy=createdAt:desc&limit=5
//    → 5 most recent users

// 4. GET /api/users?status=active&sortBy=name:asc&page=1&limit=50
//    → First 50 active users, sorted by name alphabetically
```

**SQL Example (for comparison):**

```javascript
// With Sequelize (PostgreSQL/MySQL)
const { Op } = require('sequelize');

const getAllUsers = async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const offset = (page - 1) * limit;
  
  const where = {};
  if (req.query.role) where.role = req.query.role;
  if (req.query.status) where.status = req.query.status;
  if (req.query.search) {
    where.name = { [Op.iLike]: `%${req.query.search}%` };  // LIKE query
  }
  
  const order = [];
  if (req.query.sortBy) {
    const [field, direction] = req.query.sortBy.split(':');
    order.push([field, direction.toUpperCase()]);  // [['createdAt', 'DESC']]
  }
  
  const { rows: users, count: total } = await User.findAndCountAll({
    where,
    order,
    limit,
    offset,
    attributes: { exclude: ['password'] }  // Don't return password
  });
  
  res.json({
    data: users,
    pagination: {
      page,
      limit,
      total,
      totalPages: Math.ceil(total / limit)
    }
  });
};
```

**Best Practices:**

- Always validate and sanitize query parameters
- Set maximum limit (e.g., max 100 items per page)
- Use indexes on filtered/sorted fields for performance
- Cache results for frequently accessed data
- Return pagination metadata for better UX
- Consider cursor-based pagination for large datasets

**See also:** [Q14 for database integration](#q14-how-do-you-work-with-databases-in-nodejs-compare-sql-vs-nosql-approaches), [Q16 for query optimization](#q16-how-do-you-optimize-database-queries-in-nodejs)

## Express.js & MVC Frameworks

### Q11: Explain the middleware pattern in Express.js

**A:** Middleware are functions that execute **during the request-response cycle**. They have access to three key objects:

- `req` (request) - incoming HTTP request
- `res` (response) - outgoing HTTP response
- `next` (function) - passes control to the next middleware

**Understanding the Request-Response Cycle:**

```
Client Request
      ↓
   Middleware 1 (e.g., parse JSON)
      ↓ next()
   Middleware 2 (e.g., authentication)
      ↓ next()
   Middleware 3 (e.g., logging)
      ↓ next()
   Route Handler (your endpoint logic)
      ↓ res.send() / res.json()
Client Response
```

**What is "next"?**

`next` is a **callback function** that:

1. Passes control to the next middleware in the chain
2. If not called, the request hangs (client never gets response)
3. If called with an error `next(error)`, skips to error-handling middleware

```javascript
// Example of next() in action
app.use((req, res, next) => {
  console.log('Middleware 1');
  next(); // ← MUST call this to continue to next middleware
});

app.use((req, res, next) => {
  console.log('Middleware 2');
  next(); // ← Continue to route handler
});

app.get('/users', (req, res) => {
  console.log('Route handler');
  res.json({ message: 'Users' }); // ← Sends response, cycle ends
});

// Output for GET /users:
// Middleware 1
// Middleware 2
// Route handler
```

**Types of Middleware:**

**1. Application-level middleware (app.use):**

```javascript
const express = require('express');
const app = express();

// Applies to ALL routes
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} - ${new Date()}`);
  next();
});

// Applies only to paths starting with /api
app.use('/api', (req, res, next) => {
  console.log('API request');
  next();
});
```

**2. Router-level middleware (router.use):**

```javascript
const express = require('express');
const router = express.Router();  // ← Router is a mini-application

// Middleware for ALL routes in this router
router.use((req, res, next) => {
  console.log('Router-level middleware');
  next();
});

// Routes
router.get('/users', (req, res) => {
  res.json({ users: [] });
});

router.post('/users', (req, res) => {
  res.json({ message: 'User created' });
});

// Mount router to app
app.use('/api', router);
// Now /api/users and /api/users (POST) use router middleware
```

**Difference between app and router:**

| Feature | app | router |
|---------|-----|--------|
| Purpose | Main application | Mini-application for routes |
| Creation | `express()` | `express.Router()` |
| Scope | Global | Scoped to mounted path |
| Mounting | N/A | `app.use('/path', router)` |
| Use case | Top-level config | Organize routes by feature |

**Example organizing with routers:**

```javascript
// app.js
const express = require('express');
const app = express();
const userRoutes = require('./routes/users');
const productRoutes = require('./routes/products');

// Global middleware (all routes)
app.use(express.json());
app.use(cors());

// Mount routers
app.use('/api/users', userRoutes);      // /api/users/*
app.use('/api/products', productRoutes); // /api/products/*

// routes/users.js
const router = express.Router();

// Middleware for all user routes
router.use((req, res, next) => {
  console.log('User routes middleware');
  next();
});

router.get('/', getAllUsers);      // GET /api/users
router.get('/:id', getUserById);   // GET /api/users/:id
router.post('/', createUser);      // POST /api/users

module.exports = router;
```

**3. Error-handling middleware (4 parameters!):**

```javascript
// MUST have 4 parameters: (err, req, res, next)
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode || 500).json({
    error: err.message || 'Internal Server Error'
  });
});
```

**4. Built-in middleware:**

```javascript
app.use(express.json());           // Parse JSON bodies
app.use(express.urlencoded({ extended: true })); // Parse URL-encoded
app.use(express.static('public')); // Serve static files
```

**5. Third-party middleware:**

```javascript
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');

app.use(cors());           // CORS headers
app.use(helmet());         // Security headers
app.use(morgan('combined')); // HTTP logging
```

**Route-specific middleware:**

```javascript
// Single middleware
app.get('/profile', authMiddleware, (req, res) => {
  res.json({ user: req.user });
});

// Multiple middleware (chain)
app.delete('/users/:id', 
  authMiddleware,           // First: check authentication
  authorize('admin'),       // Then: check authorization
  validateRequest,          // Then: validate request data
  deleteUser                // Finally: route handler
);
```

**Middleware execution order is CRITICAL:**

```javascript
const express = require('express');
const app = express();

// 1. Built-in middleware (first)
app.use(express.json());

// 2. Third-party middleware
app.use(cors());
app.use(helmet());

// 3. Custom middleware
app.use(logger);

// 4. Routes
app.get('/users', getAllUsers);
app.post('/users', createUser);

// 5. 404 handler (if no route matched)
app.use((req, res) => {
  res.status(404).json({ error: 'Route not found' });
});

// 6. Error handling (MUST be last!)
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});
```

**Common middleware patterns:**

**Conditional middleware:**

```javascript
const conditionalAuth = (req, res, next) => {
  if (req.path.startsWith('/public')) {
    return next(); // Skip authentication for public routes
  }
  return authMiddleware(req, res, next);
};
```

**Async middleware (with error handling):**

```javascript
const asyncMiddleware = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

app.get('/users', asyncMiddleware(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

**Middleware with configuration:**

```javascript
const rateLimiter = (max = 100) => {
  return (req, res, next) => {
    // Rate limiting logic
    if (requestCount > max) {
      return res.status(429).json({ error: 'Too many requests' });
    }
    next();
  };
};

app.use('/api/', rateLimiter(50)); // Max 50 requests
```

**Key takeaways:**

- Middleware executes in order (top to bottom)
- Always call `next()` unless sending response
- Error middleware must have 4 parameters
- Use routers to organize related routes
- Middleware can modify req/res objects for later use

**See also:** [Q40 for advanced middleware patterns](#q40-what-is-middleware-in-nodejsexpress), [Q9 for error handling](#q9-how-do-you-handle-errors-in-nodejs-rest-apis)

### Q12: What is the MVC pattern and how do you implement it in Node.js?

MVC (Model-View-Controller) separates application into three components:

**Model**: Data structure and business logic

```javascript
// models/User.js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }
}, { timestamps: true });

userSchema.methods.toJSON = function() {
  const user = this.toObject();
  delete user.password; // Don't expose password
  return user;
};

module.exports = mongoose.model('User', userSchema);
```

**Controller**: Handles requests and responses

```javascript
// controllers/userController.js
const User = require('../models/User');
const userService = require('../services/userService');

exports.createUser = async (req, res, next) => {
  try {
    const user = await userService.createUser(req.body);
    res.status(201).json(user);
  } catch (error) {
    next(error);
  }
};

exports.getAllUsers = async (req, res, next) => {
  try {
    const users = await userService.getAllUsers(req.query);
    res.json(users);
  } catch (error) {
    next(error);
  }
};
```

**View**: In REST APIs, this is typically JSON responses. In full-stack apps, could be template engines (EJS, Pug, Handlebars)

**Service Layer** (additional layer for business logic):

```javascript
// services/userService.js
const User = require('../models/User');
const bcrypt = require('bcrypt');

exports.createUser = async (userData) => {
  const hashedPassword = await bcrypt.hash(userData.password, 10);
  const user = new User({
    ...userData,
    password: hashedPassword
  });
  return await user.save();
};

exports.getAllUsers = async (filters) => {
  return await User.find(filters);
};
```

### Q13: What popular Node.js frameworks are you familiar with?

- **Express.js**: Minimalist, most popular, great flexibility
- **Nest.js**: TypeScript-based, Angular-inspired, great for enterprise apps, built-in DI
- **Fastify**: High performance, schema-based validation
- **Koa.js**: Created by Express team, modern async/await support, lightweight
- **Hapi.js**: Configuration-driven, good for enterprise
- **Sails.js**: Full MVC framework, includes ORM (Waterline)
- **Adonis.js**: Laravel-inspired, full-featured MVC framework

For REST APIs, Express.js is most common due to its simplicity and ecosystem.

## Database Integration

### Q14: How do you work with databases in Node.js? Compare SQL vs NoSQL approaches

**MongoDB (NoSQL) with Mongoose:**

```javascript
const mongoose = require('mongoose');

// Connect
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

// Schema & Model
const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true },
  posts: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Post' }]
});

const User = mongoose.model('User', userSchema);

// CRUD operations
const user = await User.create({ name: 'John', email: 'john@example.com' });
const users = await User.find({ name: /john/i });
await User.findByIdAndUpdate(id, { name: 'Jane' });
await User.findByIdAndDelete(id);

// Populate relationships
const user = await User.findById(id).populate('posts');
```

**PostgreSQL/MySQL with Sequelize (ORM):**

```javascript
const { Sequelize, DataTypes } = require('sequelize');

const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});

const User = sequelize.define('User', {
  name: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, unique: true }
});

await sequelize.sync();
const user = await User.create({ name: 'John', email: 'john@example.com' });
const users = await User.findAll({ where: { name: 'John' } });
```

**Raw SQL with pg (PostgreSQL) or mysql2:**

```javascript
const { Pool } = require('pg');

const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

const result = await pool.query('SELECT * FROM users WHERE id = $1', [userId]);
```

**Comparison:**

- **NoSQL (MongoDB)**: Flexible schema, horizontal scaling, good for unstructured data
- **SQL (PostgreSQL/MySQL)**: ACID compliance, complex queries, relationships, structured data

### Q15: How do you handle database transactions in Node.js?

**MongoDB (Mongoose) Transactions:**

```javascript
const session = await mongoose.startSession();
session.startTransaction();

try {
  const user = await User.create([{ name: 'John' }], { session });
  const account = await Account.create([{ userId: user[0]._id, balance: 1000 }], { session });
  
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
  throw error;
} finally {
  session.endSession();
}
```

**PostgreSQL (Sequelize):**

```javascript
const t = await sequelize.transaction();

try {
  const user = await User.create({ name: 'John' }, { transaction: t });
  const account = await Account.create({ userId: user.id, balance: 1000 }, { transaction: t });
  
  await t.commit();
} catch (error) {
  await t.rollback();
  throw error;
}
```

**Raw SQL:**

```javascript
const client = await pool.connect();

try {
  await client.query('BEGIN');
  await client.query('INSERT INTO users (name) VALUES ($1)', ['John']);
  await client.query('INSERT INTO accounts (user_id, balance) VALUES ($1, $2)', [userId, 1000]);
  await client.query('COMMIT');
} catch (error) {
  await client.query('ROLLBACK');
  throw error;
} finally {
  client.release();
}
```

### Q16: How do you optimize database queries in Node.js?

1. **Indexing**: Create indexes on frequently queried fields

```javascript
userSchema.index({ email: 1 });
userSchema.index({ name: 1, createdAt: -1 }); // Compound index
```

2. **Select only needed fields**:

```javascript
const users = await User.find({}).select('name email -_id');
```

3. **Pagination**: Limit results

```javascript
const users = await User.find().skip(20).limit(10);
```

4. **Lean queries** (Mongoose): Return plain JS objects instead of documents

```javascript
const users = await User.find().lean();
```

5. **Connection pooling**: Reuse connections

```javascript
const pool = new Pool({ max: 20 }); // PostgreSQL
```

6. **Aggregation pipelines** (MongoDB): Process data in database

```javascript
const stats = await User.aggregate([
  { $match: { active: true } },
  { $group: { _id: '$role', count: { $sum: 1 } } }
]);
```

7. **Caching**: Use Redis for frequently accessed data

```javascript
const redis = require('redis');
const client = redis.createClient();

const cached = await client.get(`user:${id}`);
if (cached) return JSON.parse(cached);

const user = await User.findById(id);
await client.setex(`user:${id}`, 3600, JSON.stringify(user));
```

8. **Avoid N+1 queries**: Use joins/populate

```javascript
// Bad - N+1 queries
const users = await User.find();
for (let user of users) {
  user.posts = await Post.find({ userId: user.id });
}

// Good - Single query with join
const users = await User.find().populate('posts');
```

## Async Programming

### Q17: Explain Promises vs Async/Await

**A:** **Yes, both are asynchronous constructs!** They handle asynchronous operations but use different syntax. Async/await is built on top of Promises.

**Understanding the Relationship:**

- **Promises** = The underlying mechanism for async operations
- **Async/Await** = Syntactic sugar that makes Promises easier to read and write
- An `async` function **always returns a Promise**
- `await` can only be used inside an `async` function
- Under the hood, `await` is calling `.then()` on a Promise

**Promises (Traditional Approach):**

```javascript
function getUserData(userId) {
  return fetch(`/api/users/${userId}`)  // Returns a Promise
    .then(response => response.json())   // .then() handles resolved Promise
    .then(user => {
      console.log(user);
      return user;  // Returns value wrapped in Promise
    })
    .catch(error => {  // .catch() handles rejected Promise
      console.error(error);
      throw error;
    });
}

// Using it:
getUserData(123)
  .then(user => console.log(user.name))
  .catch(err => console.error(err));
```

**Async/Await (Modern Approach):**

```javascript
async function getUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`);
    const user = await response.json();
    console.log(user);
    return user;  // Automatically wrapped in Promise
  } catch (error) {  // Catches Promise rejections
    console.error(error);
    throw error;
  }
}

// Using it - looks synchronous but is async!
const user = await getUserData(123);
console.log(user.name);
```

**They're Equivalent:**

```javascript
// These do THE SAME THING:

// Promise version
function fetchUser() {
  return fetch('/api/user')
    .then(res => res.json())
    .then(data => data.user);
}

// Async/await version
async function fetchUser() {
  const res = await fetch('/api/user');
  const data = await res.json();
  return data.user;
}

// Both return a Promise!
fetchUser().then(user => console.log(user));
```

**Key Differences:**

| Feature         | Promises                             | Async/Await                   |
|-----------------|--------------------------------------|-------------------------------|
| Syntax          | `.then().catch()`                    | `try/catch`                   |
| Readability     | Chains can get messy                 | Looks synchronous             |
| Error handling  | `.catch()` or second `.then()` param | Standard `try/catch`          |
| Debugging       | Harder (async stack traces)          | Easier (clear stack traces)   |
| Parallel ops    | `Promise.all()` directly             | Need to create Promises first |
| Browser support | ES6 (2015)                           | ES2017                        |

**Error Handling Comparison:**

```javascript
// Promises - multiple ways
fetch('/api/user')
  .then(res => res.json())
  .catch(err => console.error('Error:', err));  // Catches all errors

// Or with second parameter
fetch('/api/user')
  .then(
    res => res.json(),           // Success handler
    err => console.error(err)    // Error handler
  );

// Async/await - standard try/catch
async function getUser() {
  try {
    const res = await fetch('/api/user');
    const data = await res.json();
    return data;
  } catch (err) {
    console.error('Error:', err);  // Catches all errors
    throw err;  // Re-throw if needed
  }
}
```

**Parallel vs Sequential Execution:**

```javascript
// ❌ Sequential - slower (waits for each)
async function getDataSequential() {
  const user = await getUser(id);        // Wait 100ms
  const posts = await getPosts(id);      // Then wait 100ms
  const comments = await getComments(id); // Then wait 100ms
  return { user, posts, comments };      // Total: 300ms
}

// ✅ Parallel - faster (all at once)
async function getDataParallel() {
  const [user, posts, comments] = await Promise.all([
    getUser(id),        // All start together
    getPosts(id),       // 
    getComments(id)     //
  ]);
  return { user, posts, comments };  // Total: ~100ms
}

// Alternative parallel approach
async function getDataParallel2() {
  const userPromise = getUser(id);      // Start (don't await yet)
  const postsPromise = getPosts(id);    // Start
  const commentsPromise = getComments(id); // Start
  
  const user = await userPromise;       // Now wait for all
  const posts = await postsPromise;
  const comments = await commentsPromise;
  
  return { user, posts, comments };
}
```

**When to use which:**

**Use Promises when:**

- You need to chain multiple async operations
- Working with older codebases
- You need fine control over Promise behavior

**Use Async/Await when:**

- You want cleaner, more readable code
- You need complex error handling
- You want easier debugging
- Modern projects (it's the standard in 2026)

**Mixed approach (common):**

```javascript
async function complexOperation() {
  try {
    // Parallel operations with Promise.all
    const [users, products] = await Promise.all([
      fetch('/api/users').then(r => r.json()),
      fetch('/api/products').then(r => r.json())
    ]);
    
    // Sequential operations
    const processed = await processData(users, products);
    
    return processed;
  } catch (error) {
    console.error(error);
    throw error;
  }
}
```

**Important: Async functions always return Promises!**

```javascript
async function example() {
  return 'hello';
}

// This is equivalent to:
function example() {
  return Promise.resolve('hello');
}

// So you can use .then()
example().then(result => console.log(result)); // 'hello'

// Or await
const result = await example(); // 'hello'
```

**See also:** [Q18 for handling multiple async operations](#q18-how-do-you-handle-multiple-async-operations), [Q5 for callback hell](#q5-explain-callback-hell-and-how-to-avoid-it)

### Q18: How do you handle multiple async operations?

**Promise.all()**: Wait for all promises (fails if any fails)

```javascript
const [users, posts, comments] = await Promise.all([
  User.find(),
  Post.find(),
  Comment.find()
]);
```

**Promise.allSettled()**: Wait for all, get all results (success or failure)

```javascript
const results = await Promise.allSettled([
  fetchData1(),
  fetchData2(),
  fetchData3()
]);

results.forEach(result => {
  if (result.status === 'fulfilled') {
    console.log('Success:', result.value);
  } else {
    console.log('Error:', result.reason);
  }
});
```

**Promise.race()**: First to complete (success or failure)

```javascript
const result = await Promise.race([
  fetchFromAPI1(),
  fetchFromAPI2()
]);
```

**Promise.any()**: First successful promise

```javascript
const result = await Promise.any([
  fetchFromBackup1(),
  fetchFromBackup2(),
  fetchFromBackup3()
]);
```

**For-loop with async/await:**

```javascript
// Sequential
for (const userId of userIds) {
  await processUser(userId);
}

// Parallel with batching
const BATCH_SIZE = 10;
for (let i = 0; i < userIds.length; i += BATCH_SIZE) {
  const batch = userIds.slice(i, i + BATCH_SIZE);
  await Promise.all(batch.map(id => processUser(id)));
}
```

## Performance & Security

### Q19: How do you optimize Node.js application performance?

**1. Use clustering** (utilize all CPU cores):

```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
  const numCPUs = os.cpus().length;
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  // Start server
  app.listen(3000);
}
```

**2. Implement caching:**

```javascript
const redis = require('redis');
const client = redis.createClient();

// Cache middleware
const cache = (duration) => (req, res, next) => {
  const key = req.originalUrl;
  client.get(key, (err, data) => {
    if (data) {
      res.json(JSON.parse(data));
    } else {
      res.originalJson = res.json;
      res.json = (body) => {
        client.setex(key, duration, JSON.stringify(body));
        res.originalJson(body);
      };
      next();
    }
  });
};

router.get('/users', cache(300), getAllUsers);
```

**3. Use compression:**

```javascript
const compression = require('compression');
app.use(compression());
```

**4. Optimize database queries** (see Q16)

**5. Use async operations:** Don't block the event loop

```javascript
// Bad - synchronous
const data = fs.readFileSync('file.txt');

// Good - asynchronous
const data = await fs.promises.readFile('file.txt');
```

**6. Implement rate limiting:**

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

**7. Use HTTP/2 and keep-alive connections**

**8. Profile and monitor**: Use tools like clinic.js, PM2, New Relic

**9. Load balancing**: Use Nginx or AWS ELB to distribute traffic

### Q20: What security best practices do you follow in Node.js?

**1. Environment variables for secrets:**

```javascript
require('dotenv').config();
const dbPassword = process.env.DB_PASSWORD;
```

**2. Use helmet.js for security headers:**

```javascript
const helmet = require('helmet');
app.use(helmet());
```

**3. Input validation and sanitization:**

```javascript
const { body, validationResult } = require('express-validator');

router.post('/users',
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 8 }),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Process request
  }
);
```

**4. Prevent NoSQL injection:**

```javascript
// Use mongoose schema validation
// Sanitize user input
const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());
```

**5. Prevent SQL injection:** Use parameterized queries

```javascript
// Bad
const query = `SELECT * FROM users WHERE id = ${userId}`;

// Good
const query = 'SELECT * FROM users WHERE id = $1';
pool.query(query, [userId]);
```

**6. Implement CORS properly:**

```javascript
const cors = require('cors');
app.use(cors({
  origin: ['https://yourdomain.com'],
  credentials: true
}));
```

**7. Rate limiting** (see Q19)

**8. Keep dependencies updated:**

```bash
npm audit
npm audit fix
```

**9. Use HTTPS in production**

**10. Implement proper authentication & authorization** (see Q8)

**11. Prevent XSS attacks:**

```javascript
// Escape user input before rendering
const xss = require('xss');
const cleanInput = xss(userInput);
```

**12. Use Content Security Policy (CSP)**

**13. Implement logging and monitoring:**

```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});
```

## Docker & DevOps

### Q21: How do you containerize a Node.js application with Docker?

**Dockerfile example:**

```dockerfile
# Use official Node.js image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application code
COPY . .

# Expose port
EXPOSE 3000

# Set environment to production
ENV NODE_ENV=production

# Create non-root user
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
USER nodejs

# Start application
CMD ["node", "server.js"]
```

**docker-compose.yml for multi-container setup:**

```yaml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    depends_on:
      - db
      - redis
    restart: unless-stopped

  db:
    image: postgres:15
    environment:
      POSTGRES_DB: mydb
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres-data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres-data:
```

**Best practices:**

- Use multi-stage builds to reduce image size
- Use .dockerignore to exclude unnecessary files
- Don't run as root user
- Use specific version tags, not 'latest'
- Cache dependencies layer (COPY package*.json before COPY . .)

**Multi-stage build for smaller images:**

```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
USER node
CMD ["node", "dist/server.js"]
```

### Q22: What's your experience with AWS services for Node.js applications?

Common AWS services for Node.js:

**1. AWS Lambda**: Serverless functions

```javascript
exports.handler = async (event) => {
  const body = JSON.parse(event.body);
  
  // Process request
  const result = await processData(body);
  
  return {
    statusCode: 200,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(result)
  };
};
```

**2. API Gateway**: RESTful APIs and WebSocket APIs

**3. EC2**: Traditional virtual servers for hosting Node.js apps

**4. ECS/EKS**: Container orchestration (Docker)

**5. S3**: File storage

```javascript
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

const uploadFile = async (file) => {
  const params = {
    Bucket: 'my-bucket',
    Key: file.name,
    Body: file.data,
    ContentType: file.mimetype
  };
  
  return await s3.upload(params).promise();
};
```

**6. DynamoDB**: NoSQL database

```javascript
const AWS = require('aws-sdk');
const dynamodb = new AWS.DynamoDB.DocumentClient();

const getItem = async (id) => {
  const params = {
    TableName: 'Users',
    Key: { id }
  };
  
  return await dynamodb.get(params).promise();
};
```

**7. RDS**: Managed relational databases (PostgreSQL, MySQL)

**8. ElastiCache**: Redis/Memcached for caching

**9. CloudWatch**: Logging and monitoring

**10. SQS/SNS**: Message queuing and notifications

**11. Elastic Beanstalk**: Platform as a Service for easy deployment

### Q23: How do you implement CI/CD for Node.js applications?

**1. GitHub Actions example (.github/workflows/deploy.yml):**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test
      - run: npm run lint

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .
      - name: Push to registry
        run: |
          docker tag myapp:${{ github.sha }} registry/myapp:latest
          docker push registry/myapp:latest

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to AWS
        run: |
          aws ecs update-service --cluster my-cluster --service my-service --force-new-deployment
```

**2. GitLab CI (.gitlab-ci.yml):**

```yaml
stages:
  - test
  - build
  - deploy

test:
  stage: test
  image: node:18
  script:
    - npm ci
    - npm test
    - npm run lint

build:
  stage: build
  script:
    - docker build -t myapp:latest .
    - docker push myapp:latest

deploy:
  stage: deploy
  script:
    - kubectl set image deployment/myapp myapp=myapp:latest
  only:
    - main
```

**3. Testing strategy:**

- Unit tests (Jest, Mocha)
- Integration tests
- E2E tests (Supertest for APIs)
- Code coverage (Istanbul/nyc)
- Static analysis (ESLint, SonarQube)

## Advanced Topics

### Q24: How do you implement WebSockets in Node.js?

**Using Socket.io:**

```javascript
const express = require('express');
const http = require('http');
const socketIo = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketIo(server, {
  cors: {
    origin: "http://localhost:3000",
    methods: ["GET", "POST"]
  }
});

// Connection handling
io.on('connection', (socket) => {
  console.log('New client connected:', socket.id);
  
  // Join room
  socket.on('join-room', (roomId) => {
    socket.join(roomId);
    socket.to(roomId).emit('user-joined', socket.id);
  });
  
  // Handle messages
  socket.on('message', (data) => {
    io.to(data.roomId).emit('message', {
      userId: socket.id,
      message: data.message,
      timestamp: new Date()
    });
  });
  
  // Disconnection
  socket.on('disconnect', () => {
    console.log('Client disconnected:', socket.id);
  });
});

server.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

**Client-side:**

```javascript
const socket = io('http://localhost:3000');

socket.on('connect', () => {
  console.log('Connected to server');
  socket.emit('join-room', 'room1');
});

socket.on('message', (data) => {
  console.log('New message:', data);
});

socket.emit('message', { roomId: 'room1', message: 'Hello!' });
```

**Use cases**: Real-time chat, live notifications, collaborative editing, gaming

### Q25: How do you handle file uploads in Node.js?

**Using Multer:**

```javascript
const multer = require('multer');
const path = require('path');

// Configure storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
    cb(null, file.fieldname + '-' + uniqueSuffix + path.extname(file.originalname));
  }
});

// File filter
const fileFilter = (req, file, cb) => {
  const allowedTypes = ['image/jpeg', 'image/png', 'image/gif'];
  
  if (allowedTypes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error('Invalid file type'), false);
  }
};

// Configure multer
const upload = multer({
  storage: storage,
  fileFilter: fileFilter,
  limits: { fileSize: 5 * 1024 * 1024 } // 5MB limit
});

// Routes
router.post('/upload', upload.single('file'), (req, res) => {
  if (!req.file) {
    return res.status(400).json({ error: 'No file uploaded' });
  }
  
  res.json({
    filename: req.file.filename,
    path: req.file.path,
    size: req.file.size
  });
});

// Multiple files
router.post('/upload-multiple', upload.array('files', 5), (req, res) => {
  res.json({
    files: req.files.map(file => ({
      filename: file.filename,
      path: file.path
    }))
  });
});

// Upload to S3
const uploadToS3 = multer({
  storage: multerS3({
    s3: s3Client,
    bucket: 'my-bucket',
    key: (req, file, cb) => {
      cb(null, Date.now().toString() + '-' + file.originalname);
    }
  })
});
```

### Q26: How do you implement background jobs and task queues?

**Using Bull (Redis-based queue):**

```javascript
const Queue = require('bull');
const emailQueue = new Queue('email', {
  redis: {
    host: '127.0.0.1',
    port: 6379
  }
});

// Add job to queue
const sendWelcomeEmail = async (userId) => {
  await emailQueue.add({
    userId,
    type: 'welcome'
  }, {
    attempts: 3,
    backoff: {
      type: 'exponential',
      delay: 2000
    }
  });
};

// Process jobs
emailQueue.process(async (job) => {
  const { userId, type } = job.data;
  
  // Send email
  await sendEmail(userId, type);
  
  return { success: true };
});

// Event handlers
emailQueue.on('completed', (job, result) => {
  console.log(`Job ${job.id} completed`);
});

emailQueue.on('failed', (job, err) => {
  console.error(`Job ${job.id} failed:`, err);
});

// Scheduled/repeated jobs
emailQueue.add({
  type: 'daily-digest'
}, {
  repeat: {
    cron: '0 9 * * *' // Every day at 9 AM
  }
});
```

**Using node-cron for simple scheduling:**

```javascript
const cron = require('node-cron');

// Run every day at midnight
cron.schedule('0 0 * * *', async () => {
  console.log('Running daily cleanup task');
  await cleanupOldRecords();
});

// Run every 5 minutes
cron.schedule('*/5 * * * *', async () => {
  await checkSystemHealth();
});
```

**Use cases**: Email sending, image processing, data synchronization, report generation, cleanup tasks

### Q27: How do you implement testing in Node.js?

**Unit testing with Jest:**

```javascript
// userService.test.js
const userService = require('./userService');
const User = require('./models/User');

jest.mock('./models/User');

describe('UserService', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });
  
  describe('createUser', () => {
    it('should create a user successfully', async () => {
      const userData = { name: 'John', email: 'john@example.com' };
      const mockUser = { id: 1, ...userData };
      
      User.create = jest.fn().mockResolvedValue(mockUser);
      
      const result = await userService.createUser(userData);
      
      expect(User.create).toHaveBeenCalledWith(userData);
      expect(result).toEqual(mockUser);
    });
    
    it('should throw error if email already exists', async () => {
      User.create = jest.fn().mockRejectedValue(new Error('Email exists'));
      
      await expect(userService.createUser({}))
        .rejects.toThrow('Email exists');
    });
  });
});
```

**Integration testing with Supertest:**

```javascript
const request = require('supertest');
const app = require('./app');
const User = require('./models/User');

describe('User API', () => {
  beforeAll(async () => {
    await User.deleteMany({});
  });
  
  describe('POST /api/users', () => {
    it('should create a new user', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };
      
      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect(201);
      
      expect(response.body).toHaveProperty('id');
      expect(response.body.name).toBe(userData.name);
      expect(response.body).not.toHaveProperty('password');
    });
    
    it('should return 400 for invalid email', async () => {
      const response = await request(app)
        .post('/api/users')
        .send({ name: 'John', email: 'invalid-email' })
        .expect(400);
      
      expect(response.body).toHaveProperty('error');
    });
  });
  
  describe('GET /api/users/:id', () => {
    it('should return user by id', async () => {
      const user = await User.create({
        name: 'John',
        email: 'john@example.com'
      });
      
      const response = await request(app)
        .get(`/api/users/${user.id}`)
        .expect(200);
      
      expect(response.body.id).toBe(user.id);
    });
    
    it('should return 404 for non-existent user', async () => {
      await request(app)
        .get('/api/users/999999')
        .expect(404);
    });
  });
});
```

**Test coverage:**

```bash
npm test -- --coverage
```

**Testing best practices:**

- Write tests before or alongside code (TDD)
- Use descriptive test names
- Test edge cases and error scenarios
- Mock external dependencies
- Aim for high code coverage (>80%)
- Use setup/teardown hooks for test data
- Keep tests isolated and independent

### Q28: How do you debug Node.js applications?

**1. Built-in debugger:**

```bash
node inspect app.js
```

**2. Chrome DevTools:**

```bash
node --inspect app.js
# Then open chrome://inspect in Chrome
```

**3. VS Code debugger (launch.json):**

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Launch Program",
      "skipFiles": ["<node_internals>/**"],
      "program": "${workspaceFolder}/app.js",
      "env": {
        "NODE_ENV": "development"
      }
    }
  ]
}
```

**4. Logging:**

```javascript
// Console logging (development)
console.log('Debug:', data);
console.error('Error:', error);

// Winston (production)
const winston = require('winston');
const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

logger.info('User created', { userId: user.id });
logger.error('Database error', { error: err.message });
```

**5. Error tracking (Sentry):**

```javascript
const Sentry = require('@sentry/node');

Sentry.init({ dsn: process.env.SENTRY_DSN });

// Capture exceptions
try {
  riskyOperation();
} catch (error) {
  Sentry.captureException(error);
}
```

**6. Performance profiling:**

```bash
node --prof app.js
node --prof-process isolate-*-v8.log > processed.txt
```

**7. Memory leak detection:**

```javascript
// Monitor memory usage
setInterval(() => {
  const used = process.memoryUsage();
  console.log('Memory usage:', {
    rss: `${Math.round(used.rss / 1024 / 1024)}MB`,
    heapTotal: `${Math.round(used.heapTotal / 1024 / 1024)}MB`,
    heapUsed: `${Math.round(used.heapUsed / 1024 / 1024)}MB`
  });
}, 5000);
```

### Q29: Describe a challenging Node.js project you worked on

**A:** Prepare a real example following this structure:

- **Situation**: Describe the project and context
- **Task**: What was your responsibility?
- **Action**: What technical decisions and implementations did you make?
- **Result**: What was the outcome? Include metrics if possible

Example areas to mention:

- Scaling challenges (handling high traffic)
- Performance optimization (reduced response time by X%)
- Complex integrations (third-party APIs)
- Migration projects (PHP to Node.js)
- Microservices architecture implementation
- Real-time features (WebSockets, notifications)

### Q30: How do you stay updated with Node.js and JavaScript ecosystem?

- Follow Node.js official blog and release notes
- Read technical blogs (Medium, Dev.to, Node Weekly newsletter)
- Participate in communities (Stack Overflow, Reddit r/node, Discord servers)
- Attend conferences and meetups (NodeConf, JSConf)
- Contribute to open-source projects
- Experiment with new libraries and frameworks
- Take online courses (Udemy, Pluralsight, Frontend Masters)
- Read documentation of popular packages
- Follow industry leaders on Twitter/LinkedIn

## Additional Core Concepts & Fundamentals

### Q31: What is REPL in Node.js?

**A:** REPL stands for **Read-Eval-Print-Loop**, an interactive programming environment similar to a terminal/console where you can:

- **Read**: Takes user input
- **Eval**: Evaluates the JavaScript code
- **Print**: Outputs the result
- **Loop**: Repeats the process

```bash
# Start REPL
node

# In REPL
> console.log("Hello World")
Hello World
undefined
> 2 + 2
4
> .help  # Show REPL commands
> .exit  # Exit REPL
```

**REPL commands:**

- `.help` - Show REPL commands
- `.break` - Exit multiline expression
- `.clear` - Reset context
- `.save filename` - Save session to file
- `.load filename` - Load file into session

REPL is great for quick testing, prototyping, and debugging. Modern Node.js REPL supports:

- Top-level await
- TypeScript with `ts-node`
- Enhanced autocomplete
- Syntax highlighting

### Q32: What is the Event Emitter pattern?

**A:** EventEmitter is a core Node.js pattern for handling asynchronous events. It's the backbone of Node's event-driven architecture.

```javascript
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {}

const myEmitter = new MyEmitter();

// Register listener
myEmitter.on('event', (data) => {
  console.log('Event occurred:', data);
});

// Emit event
myEmitter.emit('event', { message: 'Hello' });

// Once listener (fires only once)
myEmitter.once('onetime', () => {
  console.log('This will fire only once');
});

// Remove listener
const callback = () => console.log('Event');
myEmitter.on('event', callback);
myEmitter.off('event', callback); // or removeListener

// Error handling
myEmitter.on('error', (err) => {
  console.error('Error occurred:', err);
});
```

**Real-world usage:**

```javascript
// HTTP server inherits from EventEmitter
const server = http.createServer();

server.on('request', (req, res) => {
  res.end('Hello');
});

server.on('connection', (socket) => {
  console.log('New connection');
});

server.listen(3000);
```

**Best practices:**

- Always handle 'error' events to prevent crashes
- Use `.once()` for one-time events to prevent memory leaks
- Remove listeners when no longer needed
- Consider using `events.once()` for promise-based event handling:

```javascript
const { once } = require('events');

async function waitForEvent() {
  const [result] = await once(myEmitter, 'event');
  console.log(result);
}
```

**Complete Client/Server Example:**

```javascript
// ==================== SERVER SIDE ====================
// orderService.js - Server that emits events
const EventEmitter = require('events');

class OrderService extends EventEmitter {
  constructor() {
    super();
    this.orders = [];
  }
  
  // Server method that emits events
  async createOrder(orderData) {
    console.log('[SERVER] Creating order...');
    
    const order = {
      id: Date.now(),
      ...orderData,
      status: 'pending',
      createdAt: new Date()
    };
    
    this.orders.push(order);
    
    // EMIT EVENT - notify all listeners
    this.emit('order:created', order);
    
    // Simulate payment processing
    setTimeout(() => {
      order.status = 'paid';
      this.emit('order:paid', order);
    }, 1000);
    
    return order;
  }
  
  async cancelOrder(orderId) {
    const order = this.orders.find(o => o.id === orderId);
    if (order) {
      order.status = 'cancelled';
      this.emit('order:cancelled', order);
    }
  }
}

// Create server instance
const orderService = new OrderService();

// ==================== CLIENT SIDE ====================
// Multiple clients can listen to the same events

// Client 1: Email Service
orderService.on('order:created', async (order) => {
  console.log('[EMAIL SERVICE] Sending confirmation email...');
  console.log(`  To: ${order.customerEmail}`);
  console.log(`  Order ID: ${order.id}`);
  // await sendEmail(order.customerEmail, 'Order Confirmation', order);
});

orderService.on('order:paid', (order) => {
  console.log('[EMAIL SERVICE] Sending payment confirmation...');
});

// Client 2: Inventory Service  
orderService.on('order:created', async (order) => {
  console.log('[INVENTORY SERVICE] Reserving items...');
  console.log(`  Items: ${JSON.stringify(order.items)}`);
  // await inventoryService.reserve(order.items);
});

orderService.on('order:cancelled', (order) => {
  console.log('[INVENTORY SERVICE] Releasing reserved items...');
  // await inventoryService.release(order.items);
});

// Client 3: Analytics Service
orderService.on('order:created', (order) => {
  console.log('[ANALYTICS] Tracking order creation event');
  // analyticsService.track('order_created', order);
});

orderService.on('order:paid', (order) => {
  console.log('[ANALYTICS] Tracking payment event');
  // analyticsService.track('order_paid', order);
});

// Client 4: Notification Service (one-time listener)
orderService.once('order:created', (order) => {
  console.log('[NOTIFICATION] First order of the session!');
  // Only fires once
});

// ==================== TRIGGER EVENTS ====================
// When server creates an order, all clients are notified automatically

(async () => {
  console.log('\\n=== Creating first order ===');
  await orderService.createOrder({
    customerEmail: 'john@example.com',
    items: ['product1', 'product2'],
    total: 99.99
  });
  
  setTimeout(async () => {
    console.log('\\n=== Creating second order ===');
    await orderService.createOrder({
      customerEmail: 'jane@example.com',
      items: ['product3'],
      total: 49.99
    });
    // Note: 'once' listener won't fire for this one
  }, 2000);
})();

/* OUTPUT:
=== Creating first order ===
[SERVER] Creating order...
[EMAIL SERVICE] Sending confirmation email...
  To: john@example.com
  Order ID: 1705594123456
[INVENTORY SERVICE] Reserving items...
  Items: ["product1","product2"]
[ANALYTICS] Tracking order creation event
[NOTIFICATION] First order of the session!

(after 1 second)
[EMAIL SERVICE] Sending payment confirmation...
[ANALYTICS] Tracking payment event

=== Creating second order ===
[SERVER] Creating order...
[EMAIL SERVICE] Sending confirmation email...
  To: jane@example.com
  Order ID: 1705594125456
[INVENTORY SERVICE] Reserving items...
  Items: ["product3"]
[ANALYTICS] Tracking order creation event
(Notice: NOTIFICATION service doesn't fire - it used .once())
*/
```

**Key Concepts Demonstrated:**

1. **Server emits events** - `orderService.emit('event', data)`
2. **Multiple clients listen** - Each service registers with `.on()`
3. **Decoupling** - Services don't know about each other
4. **One-time listeners** - `.once()` fires only once
5. **Event data** - Passed from emitter to all listeners
6. **Asynchronous** - Listeners can be async functions

**Real-world Benefits:**

- **Loose coupling**: Email service doesn't know about inventory service
- **Scalability**: Easy to add new listeners (e.g., SMS service)
- **Maintainability**: Each service has single responsibility
- **Testability**: Can test services in isolation

**See also:** [Q24 for WebSockets](#q24-how-do-you-implement-websockets-in-nodejs), [Q48 for Socket.IO](#q48-how-do-you-implement-real-time-features-with-socketio)

### Q33: What's the difference between `process.nextTick()` and `setImmediate()`?

**A:** This is a frequently misunderstood topic:

**process.nextTick():**

- Fires **before** the next event loop phase
- Executes **immediately** after the current operation
- Has higher priority than I/O events
- Can starve the event loop if used recursively

**setImmediate():**

- Fires in the **check phase** of the event loop
- Executes after I/O events
- Better for breaking up long-running operations

```javascript
console.log('1');

setImmediate(() => {
  console.log('2 - setImmediate');
});

process.nextTick(() => {
  console.log('3 - nextTick');
});

console.log('4');

// Output:
// 1
// 4
// 3 - nextTick
// 2 - setImmediate
```

**When to use what:**

- **process.nextTick()**: When you need to execute code before I/O events (rarely needed with async/await)
- **setImmediate()**: When you want to defer execution but allow I/O to complete first
- **queueMicrotask()**: Modern alternative to `nextTick` (ES2020+)
- **setTimeout(fn, 0)**: Lower priority, after timers phase

```javascript
// Modern approach - queueMicrotask
queueMicrotask(() => {
  console.log('Microtask - similar to nextTick');
});
```

### Q34: How does Node.js handle child processes?

**A:** Child processes allow Node.js to spawn **separate processes** (not threads!) for running external programs or scaling Node.js apps.

**Important**: Child processes are **multi-process**, not multi-threaded. Each child process:

- Has its own memory space (isolated)
- Has its own V8 instance
- Runs completely independently
- Can be any program (not just Node.js)

**For detailed explanation of threading vs processes, see [Understanding Workers section after Q39](#-understanding-workers-threads-and-processes-in-nodejs)**

Node.js provides the `child_process` module with 4 main methods:

**Methods:**

1. **spawn()** - For streaming data

```javascript
const { spawn } = require('child_process');

const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
  console.log(`stdout: ${data}`);
});

ls.stderr.on('data', (data) => {
  console.error(`stderr: ${data}`);
});

ls.on('close', (code) => {
  console.log(`Process exited with code ${code}`);
});

// Modern: Using promises
const { spawn } = require('child_process');
const { promisify } = require('util');
const exec = promisify(require('child_process').exec);

async function runCommand() {
  try {
    const { stdout, stderr } = await exec('ls -lh');
    console.log('stdout:', stdout);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

2. **exec()** - For small outputs (buffers entire output)

```javascript
const { exec } = require('child_process');

exec('ls -lh', (error, stdout, stderr) => {
  if (error) {
    console.error(`Error: ${error}`);
    return;
  }
  console.log(`stdout: ${stdout}`);
});
```

3. **execFile()** - Direct file execution (more efficient)

```javascript
const { execFile } = require('child_process');

execFile('node', ['--version'], (error, stdout, stderr) => {
  if (error) throw error;
  console.log(stdout);
});
```

4. **fork()** - For Node.js processes (IPC channel)

```javascript
// parent.js
const { fork } = require('child_process');

const child = fork('./child.js');

child.on('message', (msg) => {
  console.log('Message from child:', msg);
});

child.send({ hello: 'world' });

// child.js
process.on('message', (msg) => {
  console.log('Message from parent:', msg);
  process.send({ received: true });
});
```

**Best Practice - Worker Threads:**
For CPU-intensive tasks, use Worker Threads instead of child processes:

```javascript
const { Worker } = require('worker_threads');

function runWorker(data) {
  return new Promise((resolve, reject) => {
    const worker = new Worker('./worker.js', {
      workerData: data
    });
    
    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0) {
        reject(new Error(`Worker stopped with exit code ${code}`));
      }
    });
  });
}

// worker.js
const { parentPort, workerData } = require('worker_threads');

// CPU-intensive calculation
const result = heavyComputation(workerData);
parentPort.postMessage(result);
```

### Q35: What is the difference between operational and programmer errors?

**A:** This distinction is crucial for proper error handling in Node.js:

**Operational Errors** (expected, recoverable):

- Invalid user input
- Request timeout
- Network connection failure
- Database connection lost
- File not found
- Out of disk space
- Resource unavailable

**Programmer Errors** (bugs in code):

- Syntax errors
- Calling async function without callback
- Reading property of `undefined`
- Passing wrong type to function
- Memory leaks

**How to handle them:**

```javascript
// Operational errors - handle gracefully
class OperationalError extends Error {
  constructor(message) {
    super(message);
    this.name = 'OperationalError';
    this.isOperational = true;
  }
}

// API endpoint
app.get('/user/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    
    if (!user) {
      // Operational error - handle it
      throw new OperationalError('User not found');
    }
    
    res.json(user);
  } catch (error) {
    next(error);
  }
});

// Error handling middleware
app.use((err, req, res, next) => {
  if (err.isOperational) {
    // Safe to continue
    res.status(400).json({ error: err.message });
  } else {
    // Programmer error - log and crash
    console.error('FATAL ERROR:', err);
    res.status(500).json({ error: 'Internal server error' });
    
    // Graceful shutdown for programmer errors
    process.exit(1);
  }
});

// Uncaught exceptions (programmer errors)
process.on('uncaughtException', (err) => {
  console.error('UNCAUGHT EXCEPTION! Shutting down...');
  console.error(err);
  process.exit(1);
});

// Unhandled promise rejections
process.on('unhandledRejection', (err) => {
  console.error('UNHANDLED REJECTION! Shutting down...');
  console.error(err);
  process.exit(1);
});
```

**Best Practices:**

- Use error monitoring services (Sentry, Rollbar, LogRocket)
- Implement graceful shutdown
- Use process managers (PM2) for automatic restart
- Log all errors with context
- Never swallow errors silently

### Q36: What is the purpose of `module.exports` vs `exports`?

**Understanding the difference:**

```javascript
// Node.js does this behind the scenes:
var module = { exports: {} };
var exports = module.exports;

// At the end, Node returns:
return module.exports;
```

**Correct usage:**

```javascript
// ✅ CORRECT - Using module.exports
module.exports = function() {
  console.log('Hello');
};

// ✅ CORRECT - Using exports as reference
exports.foo = function() {
  console.log('foo');
};

exports.bar = function() {
  console.log('bar');
};

// ❌ WRONG - This breaks the reference
exports = function() {
  console.log('Will not work');
};

// The problem:
// exports = something; // exports now points to new object
// But Node returns module.exports, not exports!
```

**Modern ES6 modules:**

```javascript
// Use ES6 modules instead (package.json: "type": "module")

// Named exports
export const foo = () => console.log('foo');
export const bar = () => console.log('bar');

// Default export
export default function() {
  console.log('default');
}

// Import
import defaultFn, { foo, bar } from './module.js';
```

**Best practice:**

- Use ES6 modules for new projects
- Use `.mjs` extension or `"type": "module"` in package.json
- For CommonJS: Always use `module.exports` for single exports
- Use `exports.property` for multiple exports

### Q37: What is the buffer class in Node.js?

Buffer is a class for handling binary data. Before Node.js had TypedArrays, Buffer was the primary way to handle binary data.

**Creating buffers (modern way):**

```javascript
// ⚠️ OLD WAY (deprecated)
const buf1 = new Buffer('hello'); // DON'T USE

// ✅ NEW WAY (Node.js 6+)
const buf1 = Buffer.from('hello', 'utf8');
const buf2 = Buffer.from([1, 2, 3, 4]);
const buf3 = Buffer.alloc(10); // Safe, filled with zeros
const buf4 = Buffer.allocUnsafe(10); // Faster but may contain old data

// String to Buffer
const buf = Buffer.from('Hello World', 'utf8');

// Buffer to String
console.log(buf.toString('utf8')); // 'Hello World'
console.log(buf.toString('hex'));  // '48656c6c6f20576f726c64'
console.log(buf.toString('base64')); // 'SGVsbG8gV29ybGQ='
```

**Common operations:**

```javascript
// Reading/Writing
const buf = Buffer.alloc(4);
buf.writeInt32BE(0x12345678, 0);
console.log(buf.readInt32BE(0)); // 305419896

// Concatenation
const buf1 = Buffer.from('Hello ');
const buf2 = Buffer.from('World');
const buf3 = Buffer.concat([buf1, buf2]);
console.log(buf3.toString()); // 'Hello World'

// Comparison
const buf4 = Buffer.from('ABC');
const buf5 = Buffer.from('ABC');
const buf6 = Buffer.from('ABD');

console.log(buf4.equals(buf5)); // true
console.log(buf4.compare(buf6)); // -1 (buf4 < buf6)

// Copying
const target = Buffer.alloc(10);
buf1.copy(target, 0);

// Slicing
const slice = buf1.subarray(0, 5); // Modern method
// const slice = buf1.slice(0, 5); // Old method (still works)
```

**Use cases:**

1. **File operations:**

```javascript
const fs = require('fs').promises;

// Read file as buffer
const buffer = await fs.readFile('image.png');

// Write buffer to file
await fs.writeFile('copy.png', buffer);
```

2. **Network operations:**

```javascript
const net = require('net');

const server = net.createServer((socket) => {
  socket.on('data', (buffer) => {
    console.log('Received:', buffer.toString());
    socket.write(Buffer.from('Response'));
  });
});
```

3. **Cryptography:**

```javascript
const crypto = require('crypto');

const buffer = Buffer.from('sensitive data');
const hash = crypto.createHash('sha256');
hash.update(buffer);
console.log(hash.digest('hex'));
```

**Best Practices:**

- Prefer `Buffer.from()` over `new Buffer()`
- Use `Buffer.alloc()` for security (zeros memory)
- Use `Buffer.allocUnsafe()` only when performance is critical AND you immediately overwrite the data
- Consider using TypedArrays (Uint8Array) for browser compatibility
- Use streams for large files instead of loading entire buffer

**Modern alternative - TextEncoder/TextDecoder:**

```javascript
// Modern Web API (works in Node.js 11+)
const encoder = new TextEncoder();
const decoder = new TextDecoder();

const uint8Array = encoder.encode('Hello World');
const text = decoder.decode(uint8Array);
console.log(text); // 'Hello World'
```

### Q38: What are the different types of HTTP requests in Node.js?

**A:** **Modern approach with native fetch (Node.js 18+):**

Node.js 18+ includes native `fetch` API, making HTTP requests much simpler:

```javascript
// ✅ Native fetch (Node.js 18+)
async function makeRequests() {
  // GET request
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  
  // POST request
  const postResponse = await fetch('https://api.example.com/users', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer token123'
    },
    body: JSON.stringify({ name: 'John', email: 'john@example.com' })
  });
  
  // PUT request
  const putResponse = await fetch('https://api.example.com/users/1', {
    method: 'PUT',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ name: 'Jane' })
  });
  
  // DELETE request
  const deleteResponse = await fetch('https://api.example.com/users/1', {
    method: 'DELETE'
  });
  
  // PATCH request
  const patchResponse = await fetch('https://api.example.com/users/1', {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email: 'newemail@example.com' })
  });
  
  // Error handling
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  return data;
}
```

**Using http/https modules (built-in, legacy):**

```javascript
const https = require('https');

// GET request
function getData(url) {
  return new Promise((resolve, reject) => {
    https.get(url, (res) => {
      let data = '';
      
      res.on('data', (chunk) => {
        data += chunk;
      });
      
      res.on('end', () => {
        resolve(JSON.parse(data));
      });
    }).on('error', reject);
  });
}

// POST request
function postData(url, postData) {
  return new Promise((resolve, reject) => {
    const data = JSON.stringify(postData);
    
    const options = {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Content-Length': data.length
      }
    };
    
    const req = https.request(url, options, (res) => {
      let responseData = '';
      
      res.on('data', (chunk) => {
        responseData += chunk;
      });
      
      res.on('end', () => {
        resolve(JSON.parse(responseData));
      });
    });
    
    req.on('error', reject);
    req.write(data);
    req.end();
  });
}
```

**Using popular libraries:**

1. **Axios**:

```javascript
const axios = require('axios');

// GET
const response = await axios.get('https://api.example.com/data');

// POST
const response = await axios.post('https://api.example.com/users', {
  name: 'John',
  email: 'john@example.com'
});

// With configuration
const response = await axios({
  method: 'post',
  url: 'https://api.example.com/users',
  data: { name: 'John' },
  headers: { 'Authorization': 'Bearer token' },
  timeout: 5000
});

// Interceptors
axios.interceptors.request.use((config) => {
  config.headers.Authorization = `Bearer ${getToken()}`;
  return config;
});
```

2. **Got** (modern alternative):

```javascript
const got = require('got');

// GET
const response = await got('https://api.example.com/data').json();

// POST
const response = await got.post('https://api.example.com/users', {
  json: { name: 'John', email: 'john@example.com' }
}).json();
```

### Q39: How does Node.js support multi-core processors?

Node.js is single-threaded, but can leverage multiple cores using:

**1. Cluster Module (traditional approach):**

```javascript
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  console.log(`Master ${process.pid} is running`);
  
  // Fork workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
  
  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died`);
    // Restart worker
    cluster.fork();
  });
  
} else {
  // Workers share TCP connection
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('Hello World\n');
  }).listen(8000);
  
  console.log(`Worker ${process.pid} started`);
}
```

**2. Worker Threads (modern approach for CPU-intensive tasks):**

```javascript
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

if (isMainThread) {
  // Main thread
  const worker = new Worker(__filename, {
    workerData: { num: 5 }
  });
  
  worker.on('message', (result) => {
    console.log('Result from worker:', result);
  });
  
  worker.on('error', (error) => {
    console.error('Worker error:', error);
  });
  
  worker.on('exit', (code) => {
    if (code !== 0) {
      console.error(`Worker stopped with exit code ${code}`);
    }
  });
  
} else {
  // Worker thread
  const result = fibonacci(workerData.num);
  parentPort.postMessage(result);
}

function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```

**3. Worker Thread Pool (best practice):**

```javascript
const { Worker } = require('worker_threads');
const os = require('os');

class WorkerPool {
  constructor(workerPath, poolSize = os.cpus().length) {
    this.workerPath = workerPath;
    this.poolSize = poolSize;
    this.workers = [];
    this.queue = [];
    
    // Initialize workers
    for (let i = 0; i < poolSize; i++) {
      this.addWorker();
    }
  }
  
  addWorker() {
    const worker = new Worker(this.workerPath);
    worker.busy = false;
    
    worker.on('message', (result) => {
      worker.busy = false;
      worker.task.resolve(result);
      this.processQueue();
    });
    
    worker.on('error', (error) => {
      worker.task.reject(error);
      worker.busy = false;
      this.processQueue();
    });
    
    this.workers.push(worker);
  }
  
  async exec(data) {
    return new Promise((resolve, reject) => {
      this.queue.push({ data, resolve, reject });
      this.processQueue();
    });
  }
  
  processQueue() {
    if (this.queue.length === 0) return;
    
    const availableWorker = this.workers.find(w => !w.busy);
    if (!availableWorker) return;
    
    const task = this.queue.shift();
    availableWorker.busy = true;
    availableWorker.task = task;
    availableWorker.postMessage(task.data);
  }
  
  destroy() {
    this.workers.forEach(worker => worker.terminate());
  }
}

// Usage
const pool = new WorkerPool('./heavy-computation.js', 4);

async function processTasks() {
  const tasks = [1, 2, 3, 4, 5, 6, 7, 8].map(num =>
    pool.exec({ number: num })
  );
  
  const results = await Promise.all(tasks);
  console.log('Results:', results);
  
  pool.destroy();
}
```

**4. PM2 Process Manager (production standard):**

```bash
# Install PM2
npm install -g pm2

# Start app in cluster mode
pm2 start app.js -i max  # Use all CPU cores
pm2 start app.js -i 4    # Use 4 instances

# Monitor
pm2 monit

# Logs
pm2 logs

# Zero-downtime reload
pm2 reload app
```

**ecosystem.config.js for PM2:**

```javascript
module.exports = {
  apps: [{
    name: 'api',
    script: './app.js',
    instances: 'max',  // Use all CPUs
    exec_mode: 'cluster',
    max_memory_restart: '1G',
    env: {
      NODE_ENV: 'production'
    },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm Z'
  }]
};
```

**Best Practices:**

- Use Worker Threads for CPU-intensive tasks within app
- Use Cluster module or PM2 for scaling HTTP servers
- Use container orchestration (Kubernetes) for true horizontal scaling
- Consider serverless (AWS Lambda) for automatic scaling

---

## 🧵 Understanding Workers, Threads, and Processes in Node.js

**Key Question: Is Node.js multi-threaded or multi-process?**

**Answer: Node.js is SINGLE-THREADED by default, but can be BOTH multi-threaded AND multi-process!**

### The Node.js Threading Model

**1. The Main Thread (Event Loop)**

- Node.js runs JavaScript in a **single thread** called the **main thread**
- This thread executes your code and the event loop
- **Non-blocking I/O** allows handling many concurrent operations without threads
- Perfect for I/O-bound operations (file system, network, databases)

```javascript
// All this runs on ONE thread!
app.get('/users', async (req, res) => {
  const users = await User.find();  // I/O - doesn't block
  res.json(users);
});
```

**Why single-threaded works for I/O:**

```
Request 1: Read file → (waiting) → process result
Request 2:   Read DB → (waiting) → process result
Request 3:  HTTP call → (waiting) → process result

While waiting, the event loop handles other requests!
No threads needed because Node delegates I/O to the OS.
```

**2. libuv Thread Pool (Hidden Threads)**

- Node.js DOES use threads internally via **libuv**
- Default: 4 threads for file system operations and DNS lookups
- You don't control these directly
- Can configure: `process.env.UV_THREADPOOL_SIZE = 8`

**3. When Single Thread Becomes a Problem**

```javascript
// ❌ BAD - Blocks the entire server!
app.get('/compute', (req, res) => {
  let result = 0;
  for (let i = 0; i < 10_000_000_000; i++) {
    result += i;  // CPU-intensive, blocks event loop
  }
  res.json({ result });
});

// While this runs, ALL other requests wait!
```

### Solutions: Multi-Threading and Multi-Processing

#### Option 1: Worker Threads (Multi-Threading within Node)

**What are Worker Threads?**

- True threads that run JavaScript in parallel
- Share memory with main thread (via SharedArrayBuffer)
- Part of the same Node.js process
- Introduced in Node.js 10.5.0, stable in 12+

**When to use:**

- CPU-intensive JavaScript operations
- Image/video processing
- Data encryption/compression
- Complex calculations
- Machine learning inference

```javascript
// worker.js - Runs in separate thread
const { parentPort, workerData } = require('worker_threads');

// CPU-intensive calculation
function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const result = fibonacci(workerData.number);
parentPort.postMessage(result);  // Send result back to main thread
```

```javascript
// main.js - Main thread
const { Worker } = require('worker_threads');

app.get('/compute', async (req, res) => {
  const worker = new Worker('./worker.js', {
    workerData: { number: 40 }
  });
  
  worker.on('message', (result) => {
    res.json({ result });  // Main thread not blocked!
  });
  
  worker.on('error', (error) => {
    res.status(500).json({ error: error.message });
  });
});
```

**Worker Threads vs Main Thread:**
| Feature | Main Thread | Worker Thread |
|---------|-------------|---------------|
| Purpose | Event loop, I/O | CPU-intensive tasks |
| Memory | Separate heap | Can share via SharedArrayBuffer |
| Communication | N/A | Message passing (postMessage) |
| Overhead | Low | Medium (thread creation) |
| Crash impact | Entire app | Only that worker |

#### Option 2: Child Processes (Multi-Processing)

**What are Child Processes?**

- Separate Node.js processes
- Completely isolated memory
- Can run ANY program (not just Node.js)
- Higher overhead than Worker Threads

**When to use:**

- Running external commands (e.g., `ffmpeg`, `imagemagick`)
- Complete isolation needed
- Legacy scripts in other languages
- Microservices within same machine

```javascript
const { spawn } = require('child_process');

// Run external program
const ffmpeg = spawn('ffmpeg', ['-i', 'input.mp4', 'output.avi']);

ffmpeg.stdout.on('data', (data) => {
  console.log(`Progress: ${data}`);
});
```

#### Option 3: Cluster Module (Multi-Processing for HTTP)

**What is Cluster?**

- Creates multiple Node.js processes
- All share the same server port
- Load balancer built-in
- Each process is completely independent

**When to use:**

- Scaling HTTP/WebSocket servers
- Utilizing all CPU cores
- Production deployments

```javascript
const cluster = require('cluster');
const numCPUs = require('os').cpus().length;

if (cluster.isMaster) {
  // Master process creates workers
  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }
} else {
  // Worker process runs server
  app.listen(3000);
}

// Result: 8 CPU cores = 8 separate Node.js processes
// All can handle requests on port 3000!
```

### Comparison Table

| Approach | Type | Memory | Use Case | Overhead |
|----------|------|--------|----------|----------|
| Main Thread | Single | N/A | I/O operations | None |
| Worker Threads | Multi-threaded | Shared (optional) | CPU tasks (JS) | Low |
| Child Processes | Multi-process | Isolated | External programs | Medium |
| Cluster | Multi-process | Isolated | HTTP scaling | Medium |
| PM2 | Multi-process | Isolated | Production | Low (managed) |

### Real-World Architecture (2026)

**Typical production setup:**

```
Load Balancer (Nginx)
         ↓
    ┌────┴────┐
    ↓         ↓
Server 1    Server 2  (Multiple machines - horizontal scaling)
    ↓         ↓
PM2 (Cluster)  (4 processes per server)
    ↓
Each process:
 - Main thread (event loop)
 - Worker threads pool (for CPU tasks)
 - libuv threads (for file I/O)
```

### Decision Matrix

**Use Main Thread (default) when:**

- ✅ Handling HTTP requests
- ✅ Database queries
- ✅ File I/O
- ✅ Network calls
- ❌ NOT for heavy CPU computation

**Use Worker Threads when:**

- ✅ Image/video processing in Node.js
- ✅ Data encryption/compression
- ✅ Complex algorithms
- ✅ ML inference with JS libraries
- ❌ NOT for I/O operations

**Use Child Processes when:**

- ✅ Running external commands
- ✅ Spawning other programs
- ✅ Complete isolation needed
- ❌ NOT for simple calculations

**Use Cluster/PM2 when:**

- ✅ Production HTTP servers
- ✅ Need to use all CPU cores
- ✅ Zero-downtime deployments
- ✅ Auto-restart on crashes

### Summary

**Node.js is:**

1. **Single-threaded** for JavaScript execution (event loop)
2. **Multi-threaded** internally (libuv for I/O)
3. **Multi-threaded** explicitly (Worker Threads for CPU tasks)
4. **Multi-process** for scaling (Cluster, PM2)

**The beauty**: You can be all of these at once in a production app!

**See Q34 for Child Processes details** | **See Q44 for Worker Threads examples**

- Use container orchestration (Kubernetes) for true horizontal scaling
- Consider serverless (AWS Lambda) for automatic scaling

### Q40: What is middleware in Node.js/Express?

Middleware functions have access to request, response, and next function in the request-response cycle.

**Types of middleware:**

1. **Application-level middleware:**

```javascript
const express = require('express');
const app = express();

// Applies to all routes
app.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// Applies to specific path
app.use('/api', (req, res, next) => {
  console.log('API Request');
  next();
});
```

2. **Router-level middleware:**

```javascript
const router = express.Router();

router.use((req, res, next) => {
  console.log('Router middleware');
  next();
});

router.get('/users', (req, res) => {
  res.json({ users: [] });
});

app.use('/api', router);
```

3. **Error-handling middleware:**

```javascript
// Must have 4 parameters!
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode || 500).json({
    error: {
      message: err.message,
      ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
    }
  });
});
```

4. **Built-in middleware:**

```javascript
// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));

// Serve static files
app.use(express.static('public'));
```

5. **Third-party middleware:**

```javascript
const cors = require('cors');
const helmet = require('helmet');
const morgan = require('morgan');
const compression = require('compression');

app.use(cors());
app.use(helmet());
app.use(morgan('combined'));
app.use(compression());
```

**Custom middleware examples:**

**Authentication middleware:**

```javascript
const jwt = require('jsonwebtoken');

const authenticate = async (req, res, next) => {
  try {
    const token = req.headers.authorization?.replace('Bearer ', '');
    
    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }
    
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = await User.findById(decoded.userId);
    
    if (!req.user) {
      return res.status(401).json({ error: 'Invalid token' });
    }
    
    next();
  } catch (error) {
    res.status(401).json({ error: 'Authentication failed' });
  }
};

// Usage
app.get('/protected', authenticate, (req, res) => {
  res.json({ user: req.user });
});
```

**Request validation middleware:**

```javascript
const { body, validationResult } = require('express-validator');

const validateUser = [
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 8 }),
  body('name').trim().notEmpty(),
  
  (req, res, next) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    next();
  }
];

app.post('/users', validateUser, createUser);
```

**Rate limiting middleware:**

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP',
  standardHeaders: true,
  legacyHeaders: false
});

app.use('/api/', limiter);
```

**Async error handler wrapper:**

```javascript
// Wrapper to catch async errors
const asyncHandler = (fn) => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
};

// Usage
app.get('/users', asyncHandler(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

**Request logging middleware:**

```javascript
const logger = (req, res, next) => {
  const start = Date.now();
  
  // Log when response finishes
  res.on('finish', () => {
    const duration = Date.now() - start;
    console.log({
      method: req.method,
      url: req.url,
      status: res.statusCode,
      duration: `${duration}ms`,
      ip: req.ip,
      userAgent: req.get('user-agent')
    });
  });
  
  next();
};

app.use(logger);
```

**Middleware execution order (IMPORTANT):**

```javascript
const express = require('express');
const app = express();

// 1. Global middleware (first)
app.use(express.json());
app.use(cors());
app.use(helmet());

// 2. Logging
app.use(morgan('combined'));

// 3. Authentication (if needed for all routes)
// app.use(authenticate);

// 4. Routes with specific middleware
app.get('/public', (req, res) => {
  res.json({ message: 'Public route' });
});

app.get('/protected', authenticate, (req, res) => {
  res.json({ message: 'Protected route', user: req.user });
});

// 5. 404 handler (before error handler)
app.use((req, res, next) => {
  res.status(404).json({ error: 'Route not found' });
});

// 6. Error handling middleware (LAST!)
app.use((err, req, res, next) => {
  console.error(err);
  res.status(err.statusCode || 500).json({
    error: err.message || 'Internal server error'
  });
});

app.listen(3000);
```

**Advanced Patterns:**

**Middleware composition:**

```javascript
const compose = (...middlewares) => {
  return (req, res, next) => {
    const dispatch = (index) => {
      if (index >= middlewares.length) return next();
      
      const middleware = middlewares[index];
      middleware(req, res, () => dispatch(index + 1));
    };
    
    dispatch(0);
  };
};

// Usage
const authFlow = compose(
  authenticate,
  checkPermissions,
  validateRequest
);

app.post('/admin/users', authFlow, createUser);
```

**Conditional middleware:**

```javascript
const conditionalMiddleware = (condition, middleware) => {
  return (req, res, next) => {
    if (condition(req)) {
      return middleware(req, res, next);
    }
    next();
  };
};

// Usage
app.use(conditionalMiddleware(
  (req) => req.path.startsWith('/api'),
  rateLimit()
));
```

## Advanced Topics & Modern Practices

### Q41: Explain package.json and important fields

**A:** package.json is the manifest file for Node.js projects.

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My awesome application",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "lint": "eslint .",
    "build": "tsc"
  },
  "keywords": ["api", "rest", "node"],
  "author": "Your Name <email@example.com>",
  "license": "MIT",
  "engines": {
    "node": ">=18.0.0",
    "npm": ">=9.0.0"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^7.0.0"
  },
  "devDependencies": {
    "jest": "^29.5.0",
    "nodemon": "^3.0.0",
    "eslint": "^8.40.0"
  }
}
```

**Important fields:**

- **name**: Package name (lowercase, no spaces)
- **version**: Semantic versioning (MAJOR.MINOR.PATCH)
- **type**: "module" for ES6 modules, "commonjs" for CommonJS
- **main**: Entry point for CommonJS
- **engines**: Specify Node.js/npm version requirements
- **scripts**: Custom commands
- **dependencies**: Production dependencies
- **devDependencies**: Development-only dependencies

**Version ranges:**

- `^1.2.3`: Compatible with version (>=1.2.3 <2.0.0)
- `~1.2.3`: Approximately equivalent (>=1.2.3 <1.3.0)
- `1.2.3`: Exact version
- `*` or `latest`: Latest version (avoid in production)

### Q42: How do you implement TypeScript with Node.js?

**A:** **Modern TypeScript setup (2026):**

**1. Initialize project:**

```bash
npm init -y
npm install -D typescript @types/node ts-node nodemon
npx tsc --init
```

**2. tsconfig.json - Explained line by line:**

```json
{
  "compilerOptions": {
    // JavaScript Version & Modules
    "target": "ES2022",           // Output JS version (ES2022 = modern Node.js 16+)
    "module": "NodeNext",         // Module system (NodeNext = ESM + CommonJS hybrid)
    "lib": ["ES2022"],            // Include ES2022 standard library definitions
    "moduleResolution": "NodeNext", // How to resolve imports (Node.js 16+ style)
    
    // Output Directory
    "outDir": "./dist",           // Where compiled .js files go
    "rootDir": "./src",           // Where source .ts files are
    
    // Type Checking (strict mode)
    "strict": true,               // Enable ALL strict type-checking options
    /* "strict": true includes these:
       - noImplicitAny: Error on 'any' type inference
       - strictNullChecks: null/undefined must be explicit
       - strictFunctionTypes: Strict function type checking
       - strictBindCallApply: Check bind/call/apply arguments
       - strictPropertyInitialization: Class properties must be initialized
       - noImplicitThis: Error on 'this' with implicit 'any'
       - alwaysStrict: Use 'use strict' in output
    */
    
    // Module Interoperability
    "esModuleInterop": true,      // Allow 'import x from y' for CommonJS modules
    "allowSyntheticDefaultImports": true, // Allow default imports from modules without default export
    
    // Compiler Checks
    "skipLibCheck": true,         // Skip type checking of .d.ts files (faster compile)
    "forceConsistentCasingInFileNames": true, // Error on case-sensitive imports
    
    // JSON & Source Maps
    "resolveJsonModule": true,    // Allow importing .json files
    "declaration": true,          // Generate .d.ts declaration files
    "declarationMap": true,       // Generate .d.ts.map for declaration files
    "sourceMap": true,            // Generate .js.map for debugging
    
    // Additional Useful Options
    "noUnusedLocals": true,       // Error on unused local variables
    "noUnusedParameters": true,   // Error on unused function parameters
    "noImplicitReturns": true,    // Error on functions with inconsistent returns
    "noFallthroughCasesInSwitch": true // Error on switch fallthrough
  },
  "include": ["src/**/*"],        // Which files to compile
  "exclude": ["node_modules", "dist"] // Which to ignore
}
```

**What each setting does:**

| Setting | Purpose | Example |
|---------|---------|---------|
| `target` | JS version for output | ES2022 = modern features like top-level await |
| `module` | Module system | NodeNext = works with both ESM and CommonJS |
| `outDir` | Output folder | dist/ - keeps source and build separate |
| `strict` | Type safety | Catches bugs at compile time |
| `esModuleInterop` | Import compatibility | `import express from 'express'` works |
| `sourceMap` | Debugging | Maps compiled JS back to original TS |

**3. Example with Express:**

```typescript
// src/types.ts
export interface User {
  id: string;
  name: string;
  email: string;
}

export interface CreateUserDTO {
  name: string;
  email: string;
  password: string;
}

// src/controllers/UserController.ts
import { Request, Response, NextFunction } from 'express';
import { User, CreateUserDTO } from '../types';

export class UserController {
  async getUsers(req: Request, res: Response, next: NextFunction): Promise<void> {
    try {
      const users: User[] = await userService.findAll();
      res.json(users);
    } catch (error) {
      next(error);
    }
  }
  
  async createUser(
    req: Request<{}, {}, CreateUserDTO>, // <Params, ResBody, ReqBody>
    res: Response,
    next: NextFunction
  ): Promise<void> {
    try {
      const user = await userService.create(req.body);
      res.status(201).json(user);
    } catch (error) {
      next(error);
    }
  }
}

// src/app.ts
import express, { Express } from 'express';
import { UserController } from './controllers/UserController';

const app: Express = express();
const userController = new UserController();

app.use(express.json());

app.get('/users', userController.getUsers);
app.post('/users', userController.createUser);

export default app;
```

**4. package.json scripts:**

```json
{
  "scripts": {
    "build": "tsc",                           // Compile TS to JS
    "start": "node dist/index.js",            // Run compiled JS
    "dev": "nodemon --exec ts-node src/index.ts", // Dev with hot reload
    "watch": "tsc --watch",                   // Auto-recompile on changes
    "type-check": "tsc --noEmit"              // Check types without compiling
  }
}
```

**Common tsconfig.json Configurations:**

**For Strict Projects (Recommended):**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,                    // All strict checks
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true  // Safer array/object access
  }
}
```

**For Legacy/Migration Projects:**

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "strict": false,                   // Gradual adoption
    "noImplicitAny": false,           // Allow 'any' for now
    "allowJs": true,                  // Mix JS and TS files
    "checkJs": false                  // Don't check JS files
  }
}
```

**For Libraries:**

```json
{
  "compilerOptions": {
    "declaration": true,              // Generate .d.ts files
    "declarationMap": true,
    "sourceMap": true,
    "outDir": "./dist",
    "removeComments": true,           // Smaller output
    "importHelpers": true             // Use tslib for helpers
  }
}
```

**2026 Best Practices:**

- Always use `"strict": true` for new projects
- Use `NodeNext` for module/moduleResolution (best compatibility)
- Enable source maps for easier debugging
- Define interfaces for all data structures
- Use generics for reusable code
- Add path aliases for cleaner imports:

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"],
      "@controllers/*": ["src/controllers/*"],
      "@models/*": ["src/models/*"]
    }
  }
}

// Now you can do:
import { UserController } from '@controllers/UserController';
// Instead of:
import { UserController } from '../../../controllers/UserController';
```

**See also:** [Q14 for database types](#q14-how-do-you-work-with-databases-in-nodejs-compare-sql-vs-nosql-approaches)
"skipLibCheck": true,
"forceConsistentCasingInFileNames": true,
"resolveJsonModule": true,
"declaration": true,
"declarationMap": true,
"sourceMap": true
},
"include": ["src/**/*"],
"exclude": ["node_modules", "dist"]
}

```

**3. Example with Express:**

```typescript
// src/types.ts
export interface User {
  id: string;
  name: string;
  email: string;
}

export interface CreateUserDTO {
  name: string;
  email: string;
  password: string;
}

// src/controllers/UserController.ts
import { Request, Response, NextFunction } from 'express';
import { User, CreateUserDTO } from '../types';

export class UserController {
  async getUsers(req: Request, res: Response, next: NextFunction): Promise<void> {
    try {
      const users: User[] = await userService.findAll();
      res.json(users);
    } catch (error) {
      next(error);
    }
  }
  
  async createUser(
    req: Request<{}, {}, CreateUserDTO>,
    res: Response,
    next: NextFunction
  ): Promise<void> {
    try {
      const user = await userService.create(req.body);
      res.status(201).json(user);
    } catch (error) {
      next(error);
    }
  }
}

// src/app.ts
import express, { Express } from 'express';
import { UserController } from './controllers/UserController';

const app: Express = express();
const userController = new UserController();

app.use(express.json());

app.get('/users', userController.getUsers);
app.post('/users', userController.createUser);

export default app;
```

**4. package.json scripts:**

```json
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js",
    "dev": "nodemon --exec ts-node src/index.ts",
    "watch": "tsc --watch",
    "type-check": "tsc --noEmit"
  }
}
```

**Best Practices:**

- Use strict mode
- Define interfaces for all data structures
- Use generics for reusable code
- Enable source maps for debugging
- Use path aliases for cleaner imports

### Q43: How do you implement logging in Node.js?

**Modern logging strategies:**

**1. Winston (production-grade):**

```javascript
import winston from 'winston';

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  defaultMeta: { service: 'user-service' },
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Console for development
if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.combine(
      winston.format.colorize(),
      winston.format.simple()
    )
  }));
}

// Usage
logger.info('User created', { userId: user.id, email: user.email });
logger.error('Database error', { error: error.message, stack: error.stack });
logger.warn('High memory usage', { memory: process.memoryUsage() });
```

**2. Pino (fastest):**

```javascript
import pino from 'pino';

const logger = pino({
  level: process.env.LOG_LEVEL || 'info',
  transport: {
    target: 'pino-pretty',
    options: {
      colorize: true,
      translateTime: 'SYS:standard',
      ignore: 'pid,hostname'
    }
  }
});

logger.info({ userId: '123' }, 'User logged in');
```

**3. Request logging middleware:**

```javascript
import morgan from 'morgan';
import { v4 as uuidv4 } from 'uuid';

// Add request ID
app.use((req, res, next) => {
  req.id = uuidv4();
  res.setHeader('X-Request-ID', req.id);
  next();
});

// Morgan with custom format
app.use(morgan(':method :url :status :response-time ms - :res[content-length]', {
  stream: {
    write: (message) => logger.info(message.trim())
  }
}));

// Or custom logger
app.use((req, res, next) => {
  const start = Date.now();
  
  res.on('finish', () => {
    logger.info({
      requestId: req.id,
      method: req.method,
      url: req.url,
      status: res.statusCode,
      duration: Date.now() - start,
      userAgent: req.get('user-agent'),
      ip: req.ip
    });
  });
  
  next();
});
```

**Best Practices:**

- Use structured logging (JSON)
- Include request IDs for tracing
- Log to centralized system (ELK, Datadog, CloudWatch)
- Don't log sensitive data (passwords, tokens)
- Use appropriate log levels
- Rotate log files

### Q44: What are Worker Threads and when should you use them?

**A:** **Modern Worker Threads usage:**

Worker Threads allow true parallelism in Node.js for CPU-intensive tasks.

**When to use:**

- Image/video processing
- Data encryption/decryption
- Complex calculations
- Data parsing/transformation
- Machine learning inference

**Basic example:**

```javascript
// main.js
import { Worker } from 'worker_threads';

function runWorker(data) {
  return new Promise((resolve, reject) => {
    const worker = new Worker('./worker.js', {
      workerData: data
    });
    
    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0) {
        reject(new Error(`Worker stopped with exit code ${code}`));
      }
    });
  });
}

// Usage
const result = await runWorker({ number: 40 });
console.log('Result:', result);

// worker.js
import { parentPort, workerData } from 'worker_threads';

function fibonacci(n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

const result = fibonacci(workerData.number);
parentPort.postMessage(result);
```

**Worker Pool implementation:**

```javascript
import { Worker } from 'worker_threads';
import os from 'os';

class WorkerPool {
  constructor(workerPath, poolSize = os.cpus().length) {
    this.workerPath = workerPath;
    this.workers = [];
    this.queue = [];
    
    for (let i = 0; i < poolSize; i++) {
      this.addWorker();
    }
  }
  
  addWorker() {
    const worker = new Worker(this.workerPath);
    worker.busy = false;
    
    worker.on('message', (result) => {
      worker.busy = false;
      worker.task.resolve(result);
      this.processQueue();
    });
    
    worker.on('error', (error) => {
      worker.task.reject(error);
      worker.busy = false;
      this.processQueue();
    });
    
    this.workers.push(worker);
  }
  
  async exec(data) {
    return new Promise((resolve, reject) => {
      this.queue.push({ data, resolve, reject });
      this.processQueue();
    });
  }
  
  processQueue() {
    if (this.queue.length === 0) return;
    
    const availableWorker = this.workers.find(w => !w.busy);
    if (!availableWorker) return;
    
    const task = this.queue.shift();
    availableWorker.busy = true;
    availableWorker.task = task;
    availableWorker.postMessage(task.data);
  }
  
  async destroy() {
    await Promise.all(this.workers.map(w => w.terminate()));
  }
}

// Usage
const pool = new WorkerPool('./worker.js', 4);

const tasks = Array.from({ length: 100 }, (_, i) =>
  pool.exec({ number: i })
);

const results = await Promise.all(tasks);
pool.destroy();
```

**Best Practices:**

- Don't use for I/O operations (use async/await instead)
- Pool workers for repeated tasks
- Share memory with SharedArrayBuffer when appropriate
- Monitor worker health and restart if needed
- Use worker threads over child processes for Node.js code

### Q45: How do you implement caching strategies in Node.js?

**1. In-memory caching (node-cache):**

```javascript
import NodeCache from 'node-cache';

const cache = new NodeCache({
  stdTTL: 600,  // 10 minutes default TTL
  checkperiod: 120  // Check for expired keys every 2 minutes
});

// Set cache
cache.set('user:123', userData, 3600);  // 1 hour

// Get cache
const user = cache.get('user:123');

// Delete
cache.del('user:123');

// Check existence
if (cache.has('user:123')) {
  // ...
}

// Get multiple
const values = cache.mget(['key1', 'key2', 'key3']);

// Clear all
cache.flushAll();
```

**2. Redis caching:**

```javascript
import { createClient } from 'redis';

const redisClient = await createClient({
  url: process.env.REDIS_URL
}).connect();

// Set with expiration
await redisClient.setEx('user:123', 3600, JSON.stringify(userData));

// Get
const cached = await redisClient.get('user:123');
const user = cached ? JSON.parse(cached) : null;

// Delete
await redisClient.del('user:123');

// Hash operations (for objects)
await redisClient.hSet('user:123', {
  name: 'John',
  email: 'john@example.com'
});

const user = await redisClient.hGetAll('user:123');

// Lists (for queues)
await redisClient.lPush('queue', JSON.stringify(task));
const task = await redisClient.rPop('queue');
```

**3. Cache middleware:**

```javascript
const cacheMiddleware = (duration = 300) => {
  return async (req, res, next) => {
    if (req.method !== 'GET') {
      return next();
    }
    
    const key = `cache:${req.originalUrl}`;
    
    try {
      const cached = await redisClient.get(key);
      
      if (cached) {
        return res.json(JSON.parse(cached));
      }
      
      // Capture response
      const originalJson = res.json.bind(res);
      res.json = (data) => {
        redisClient.setEx(key, duration, JSON.stringify(data));
        return originalJson(data);
      };
      
      next();
    } catch (error) {
      next();
    }
  };
};

// Usage
app.get('/users', cacheMiddleware(300), async (req, res) => {
  const users = await User.find();
  res.json(users);
});
```

**4. Cache-aside pattern:**

```javascript
async function getUser(userId) {
  // Try cache first
  const cacheKey = `user:${userId}`;
  const cached = await redisClient.get(cacheKey);
  
  if (cached) {
    return JSON.parse(cached);
  }
  
  // Cache miss - fetch from database
  const user = await User.findById(userId);
  
  if (user) {
    // Store in cache
    await redisClient.setEx(cacheKey, 3600, JSON.stringify(user));
  }
  
  return user;
}
```

**5. Multi-level caching:**

```javascript
class CacheService {
  constructor() {
    this.memoryCache = new NodeCache({ stdTTL: 60 });
    this.redisClient = redisClient;
  }
  
  async get(key) {
    // L1: Memory cache
    let value = this.memoryCache.get(key);
    if (value) return value;
    
    // L2: Redis cache
    const cached = await this.redisClient.get(key);
    if (cached) {
      value = JSON.parse(cached);
      this.memoryCache.set(key, value);
      return value;
    }
    
    return null;
  }
  
  async set(key, value, ttl = 3600) {
    this.memoryCache.set(key, value);
    await this.redisClient.setEx(key, ttl, JSON.stringify(value));
  }
  
  async delete(key) {
    this.memoryCache.del(key);
    await this.redisClient.del(key);
  }
}
```

**Best Practices:**

- Use Redis for distributed caching
- Implement cache warming for critical data
- Set appropriate TTLs
- Handle cache stampede (multiple simultaneous cache misses)
- Monitor cache hit rates
- Use cache versioning for updates

### Q46: How do you handle file uploads securely?

**1. Using Multer with validation:**

```javascript
import multer from 'multer';
import path from 'path';
import crypto from 'crypto';

// Configure storage
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    const uniqueName = crypto.randomBytes(16).toString('hex');
    const ext = path.extname(file.originalname);
    cb(null, `${uniqueName}${ext}`);
  }
});

// File filter (whitelist)
const fileFilter = (req, file, cb) => {
  const allowedMimes = [
    'image/jpeg',
    'image/png',
    'image/gif',
    'application/pdf'
  ];
  
  if (allowedMimes.includes(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error('Invalid file type'), false);
  }
};

const upload = multer({
  storage: storage,
  fileFilter: fileFilter,
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB
    files: 5
  }
});

// Routes
app.post('/upload', upload.single('file'), async (req, res, next) => {
  try {
    if (!req.file) {
      return res.status(400).json({ error: 'No file uploaded' });
    }
    
    // Additional validation
    await validateFile(req.file);
    
    // Store file metadata in database
    const fileRecord = await File.create({
      originalName: req.file.originalname,
      filename: req.file.filename,
      mimetype: req.file.mimetype,
      size: req.file.size,
      userId: req.user.id
    });
    
    res.json({
      id: fileRecord.id,
      url: `/files/${req.file.filename}`
    });
  } catch (error) {
    next(error);
  }
});
```

**2. Virus scanning:**

```javascript
import NodeClam from 'clamscan';

const ClamScan = await new NodeClam().init({
  removeInfected: true,
  scanRecursively: true
});

async function scanFile(filePath) {
  const { isInfected, viruses } = await ClamScan.isInfected(filePath);
  
  if (isInfected) {
    throw new Error(`File is infected: ${viruses.join(', ')}`);
  }
}

// In upload handler
app.post('/upload', upload.single('file'), async (req, res, next) => {
  try {
    await scanFile(req.file.path);
    // Continue processing
  } catch (error) {
    // Delete infected file
    await fs.unlink(req.file.path);
    next(error);
  }
});
```

**3. Upload to S3:**

```javascript
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import multerS3 from 'multer-s3';

const s3Client = new S3Client({ region: 'us-east-1' });

const upload = multer({
  storage: multerS3({
    s3: s3Client,
    bucket: process.env.S3_BUCKET,
    metadata: (req, file, cb) => {
      cb(null, { fieldName: file.fieldname });
    },
    key: (req, file, cb) => {
      const uniqueName = crypto.randomBytes(16).toString('hex');
      const ext = path.extname(file.originalname);
      cb(null, `uploads/${uniqueName}${ext}`);
    }
  }),
  fileFilter: fileFilter,
  limits: { fileSize: 5 * 1024 * 1024 }
});

// Generate pre-signed URL for client-side upload
app.get('/upload-url', async (req, res) => {
  const key = `uploads/${crypto.randomBytes(16).toString('hex')}.jpg`;
  
  const command = new PutObjectCommand({
    Bucket: process.env.S3_BUCKET,
    Key: key,
    ContentType: 'image/jpeg'
  });
  
  const url = await getSignedUrl(s3Client, command, { expiresIn: 3600 });
  
  res.json({ url, key });
});
```

**4. Image processing:**

```javascript
import sharp from 'sharp';

async function processImage(file) {
  const filename = `processed-${file.filename}`;
  
  await sharp(file.path)
    .resize(800, 600, { fit: 'inside' })
    .jpeg({ quality: 80 })
    .toFile(`uploads/${filename}`);
  
  // Create thumbnail
  await sharp(file.path)
    .resize(200, 200, { fit: 'cover' })
    .jpeg({ quality: 70 })
    .toFile(`uploads/thumb-${filename}`);
  
  // Delete original
  await fs.unlink(file.path);
  
  return filename;
}
```

**Security Best Practices:**

- Validate file type (check magic bytes, not just extension)
- Scan for viruses
- Limit file size
- Generate random filenames
- Store files outside web root
- Use CDN for serving files
- Implement rate limiting on upload endpoints
- Use pre-signed URLs for large files
- Process images to remove EXIF data
- Implement access control

### Q47: How do you implement rate limiting in Node.js?

**1. express-rate-limit (simple):**

```javascript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);

// Different limits for different endpoints
const strictLimiter = rateLimit({
  windowMs: 60 * 1000,
  max: 5
});

app.post('/api/login', strictLimiter, loginHandler);
```

**2. Token bucket algorithm:**

```javascript
class TokenBucket {
  constructor(capacity, refillRate) {
    this.capacity = capacity;
    this.tokens = capacity;
    this.refillRate = refillRate; // tokens per second
    this.lastRefill = Date.now();
  }
  
  refill() {
    const now = Date.now();
    const timePassed = (now - this.lastRefill) / 1000;
    const tokensToAdd = timePassed * this.refillRate;
    
    this.tokens = Math.min(this.capacity, this.tokens + tokensToAdd);
    this.lastRefill = now;
  }
  
  consume(tokens = 1) {
    this.refill();
    
    if (this.tokens >= tokens) {
      this.tokens -= tokens;
      return true;
    }
    
    return false;
  }
}

const buckets = new Map();

const tokenBucketLimiter = (req, res, next) => {
  const key = req.ip;
  
  if (!buckets.has(key)) {
    buckets.set(key, new TokenBucket(10, 1)); // 10 tokens, refill 1/sec
  }
  
  const bucket = buckets.get(key);
  
  if (bucket.consume(1)) {
    next();
  } else {
    res.status(429).json({ error: 'Rate limit exceeded' });
  }
};
```

**3. Redis-based distributed rate limiting:**

```javascript
class RedisRateLimiter {
  constructor(redisClient) {
    this.redis = redisClient;
  }
  
  async isAllowed(key, max, windowMs) {
    const now = Date.now();
    const windowStart = now - windowMs;
    
    // Sliding window algorithm
    const multi = this.redis.multi();
    
    // Remove old entries
    multi.zRemRangeByScore(key, 0, windowStart);
    
    // Count current requests
    multi.zCard(key);
    
    // Add current request
    multi.zAdd(key, now, `${now}-${Math.random()}`);
    
    // Set expiration
    multi.expire(key, Math.ceil(windowMs / 1000));
    
    const results = await multi.exec();
    const count = results[1][1];
    
    return count < max;
  }
}

const rateLimiter = new RedisRateLimiter(redisClient);

const redisLimiterMiddleware = (max, windowMs) => {
  return async (req, res, next) => {
    const key = `rate_limit:${req.ip}:${req.path}`;
    
    const allowed = await rateLimiter.isAllowed(key, max, windowMs);
    
    if (allowed) {
      next();
    } else {
      res.status(429).json({ error: 'Rate limit exceeded' });
    }
  };
};

app.use('/api/', redisLimiterMiddleware(100, 15 * 60 * 1000));
```

**4. User-based rate limiting:**

```javascript
const userRateLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: async (req) => {
    // Different limits based on user tier
    if (req.user?.tier === 'premium') {
      return 1000;
    } else if (req.user?.tier === 'basic') {
      return 100;
    }
    return 50; // Anonymous
  },
  keyGenerator: (req) => {
    return req.user?.id || req.ip;
  }
});
```

**5. Weighted rate limiting:**

```javascript
const weightedLimiter = (req, res, next) => {
  const weights = {
    'GET': 1,
    'POST': 5,
    'PUT': 5,
    'DELETE': 10
  };
  
  const weight = weights[req.method] || 1;
  const key = `rate_limit:${req.ip}`;
  
  // Use token bucket with weighted consumption
  const bucket = getBucket(key);
  
  if (bucket.consume(weight)) {
    next();
  } else {
    res.status(429).json({ error: 'Rate limit exceeded' });
  }
};
```

**Best Practices:**

- Use Redis for distributed rate limiting
- Implement sliding window algorithm
- Return appropriate headers (X-RateLimit-*)
- Use different limits for different endpoints
- Consider user tiers/subscriptions
- Implement graceful degradation
- Monitor rate limit hits
- Use CDN-level rate limiting when possible

### Q48: How do you implement real-time features with Socket.IO?

**1. Basic setup:**

```javascript
import express from 'express';
import { createServer } from 'http';
import { Server } from 'socket.io';

const app = express();
const httpServer = createServer(app);

const io = new Server(httpServer, {
  cors: {
    origin: process.env.CLIENT_URL,
    credentials: true
  },
  transports: ['websocket', 'polling']
});

// Middleware
io.use(async (socket, next) => {
  const token = socket.handshake.auth.token;
  
  try {
    const user = await verifyToken(token);
    socket.user = user;
    next();
  } catch (error) {
    next(new Error('Authentication error'));
  }
});

// Connection handling
io.on('connection', (socket) => {
  console.log('User connected:', socket.user.id);
  
  // Join user-specific room
  socket.join(`user:${socket.user.id}`);
  
  // Handle events
  socket.on('message', async (data) => {
    // Broadcast to room
    io.to(data.roomId).emit('message', {
      id: generateId(),
      text: data.text,
      userId: socket.user.id,
      timestamp: new Date()
    });
    
    // Save to database
    await Message.create(data);
  });
  
  socket.on('typing', (data) => {
    socket.to(data.roomId).emit('typing', {
      userId: socket.user.id,
      isTyping: data.isTyping
    });
  });
  
  socket.on('disconnect', (reason) => {
    console.log('User disconnected:', socket.user.id, reason);
  });
});

httpServer.listen(3000);
```

**2. Chat application:**

```javascript
class ChatService {
  constructor(io) {
    this.io = io;
  }
  
  async handleConnection(socket) {
    const userId = socket.user.id;
    
    // Join user's rooms
    const rooms = await this.getUserRooms(userId);
    rooms.forEach(room => socket.join(room.id));
    
    // Send user online
    this.io.emit('user:online', { userId });
    
    // Handle room operations
    socket.on('room:join', async (roomId) => {
      await this.joinRoom(socket, roomId);
    });
    
    socket.on('room:leave', async (roomId) => {
      await this.leaveRoom(socket, roomId);
    });
    
    // Handle messages
    socket.on('message:send', async (data) => {
      await this.sendMessage(socket, data);
    });
    
    socket.on('message:edit', async (data) => {
      await this.editMessage(socket, data);
    });
    
    socket.on('message:delete', async (data) => {
      await this.deleteMessage(socket, data);
    });
    
    // Handle disconnect
    socket.on('disconnect', async () => {
      this.io.emit('user:offline', { userId });
    });
  }
  
  async sendMessage(socket, { roomId, text, attachments }) {
    const message = await Message.create({
      roomId,
      userId: socket.user.id,
      text,
      attachments
    });
    
    this.io.to(roomId).emit('message:new', message);
  }
  
  async getUserRooms(userId) {
    return await Room.find({ members: userId });
  }
}

const chatService = new ChatService(io);

io.on('connection', (socket) => {
  chatService.handleConnection(socket);
});
```

**3. Redis adapter for scaling:**

```javascript
import { createAdapter } from '@socket.io/redis-adapter';
import { createClient } from 'redis';

const pubClient = createClient({ url: process.env.REDIS_URL });
const subClient = pubClient.duplicate();

await Promise.all([pubClient.connect(), subClient.connect()]);

io.adapter(createAdapter(pubClient, subClient));
```

**4. Notifications system:**

```javascript
class NotificationService {
  constructor(io) {
    this.io = io;
  }
  
  async sendToUser(userId, notification) {
    // Save to database
    await Notification.create({
      userId,
      ...notification
    });
    
    // Send real-time notification
    this.io.to(`user:${userId}`).emit('notification', notification);
  }
  
  async sendToRoom(roomId, notification) {
    this.io.to(roomId).emit('notification', notification);
  }
  
  async broadcast(notification) {
    this.io.emit('notification', notification);
  }
}

// Usage
const notificationService = new NotificationService(io);

// When order is created
await notificationService.sendToUser(userId, {
  type: 'order_created',
  title: 'Order Confirmed',
  message: 'Your order has been confirmed',
  data: { orderId: order.id }
});
```

**5. Presence tracking:**

```javascript
class PresenceManager {
  constructor(io, redisClient) {
    this.io = io;
    this.redis = redisClient;
  }
  
  async userConnected(userId, socketId) {
    await this.redis.sAdd(`user:${userId}:sockets`, socketId);
    await this.redis.hSet('presence', userId, 'online');
    
    this.io.emit('presence:update', {
      userId,
      status: 'online'
    });
  }
  
  async userDisconnected(userId, socketId) {
    await this.redis.sRem(`user:${userId}:sockets`, socketId);
    
    const sockets = await this.redis.sCard(`user:${userId}:sockets`);
    
    if (sockets === 0) {
      await this.redis.hSet('presence', userId, 'offline');
      
      this.io.emit('presence:update', {
        userId,
        status: 'offline',
        lastSeen: new Date()
      });
    }
  }
  
  async getUserStatus(userId) {
    return await this.redis.hGet('presence', userId);
  }
}
```

**Best Practices:**

- Use Redis adapter for horizontal scaling
- Implement authentication middleware
- Handle reconnection gracefully
- Use rooms for targeted communication
- Implement presence tracking
- Add rate limiting for events
- Use acknowledgments for critical messages
- Monitor connection count and event frequency

### Q49: How do you implement testing strategies in Node.js?

**1. Unit testing with Jest:**

```javascript
// userService.test.js
import { UserService } from './userService';
import { UserRepository } from './userRepository';

jest.mock('./userRepository');

describe('UserService', () => {
  let userService;
  let userRepository;
  
  beforeEach(() => {
    userRepository = new UserRepository();
    userService = new UserService(userRepository);
  });
  
  afterEach(() => {
    jest.clearAllMocks();
  });
  
  describe('createUser', () => {
    it('should create a user successfully', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };
      
      const expectedUser = { id: '1', ...userData };
      userRepository.create.mockResolvedValue(expectedUser);
      
      const result = await userService.createUser(userData);
      
      expect(userRepository.create).toHaveBeenCalledWith(
        expect.objectContaining({
          name: userData.name,
          email: userData.email
        })
      );
      expect(result).toEqual(expectedUser);
    });
    
    it('should throw error if email already exists', async () => {
      userRepository.findByEmail.mockResolvedValue({ id: '1' });
      
      await expect(
        userService.createUser({ email: 'existing@example.com' })
      ).rejects.toThrow('Email already exists');
    });
  });
});
```

**2. Integration testing with Supertest:**

```javascript
// api.test.js
import request from 'supertest';
import app from './app';
import { connectDB, closeDB } from './testDb';

beforeAll(async () => {
  await connectDB();
});

afterAll(async () => {
  await closeDB();
});

describe('User API', () => {
  let authToken;
  
  beforeEach(async () => {
    // Setup: Create test user and get auth token
    const response = await request(app)
      .post('/api/auth/register')
      .send({
        name: 'Test User',
        email: 'test@example.com',
        password: 'password123'
      });
    
    authToken = response.body.token;
  });
  
  describe('GET /api/users', () => {
    it('should return list of users', async () => {
      const response = await request(app)
        .get('/api/users')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);
      
      expect(response.body).toHaveProperty('users');
      expect(Array.isArray(response.body.users)).toBe(true);
    });
    
    it('should return 401 without auth token', async () => {
      await request(app)
        .get('/api/users')
        .expect(401);
    });
  });
  
  describe('POST /api/users', () => {
    it('should create a new user', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };
      
      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${authToken}`)
        .send(userData)
        .expect(201);
      
      expect(response.body).toHaveProperty('id');
      expect(response.body.name).toBe(userData.name);
      expect(response.body).not.toHaveProperty('password');
    });
    
    it('should validate required fields', async () => {
      const response = await request(app)
        .post('/api/users')
        .set('Authorization', `Bearer ${authToken}`)
        .send({ name: 'John' })
        .expect(400);
      
      expect(response.body).toHaveProperty('errors');
    });
  });
});
```

**3. E2E testing with Playwright:**

```javascript
// e2e/auth.spec.js
import { test, expect } from '@playwright/test';

test.describe('Authentication', () => {
  test('should register and login successfully', async ({ page }) => {
    // Register
    await page.goto('http://localhost:3000/register');
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.fill('[name="confirmPassword"]', 'password123');
    await page.click('button[type="submit"]');
    
    // Should redirect to dashboard
    await expect(page).toHaveURL('http://localhost:3000/dashboard');
    
    // Logout
    await page.click('[data-testid="logout-button"]');
    
    // Login
    await page.goto('http://localhost:3000/login');
    await page.fill('[name="email"]', 'test@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    
    await expect(page).toHaveURL('http://localhost:3000/dashboard');
  });
});
```

**4. Load testing with k6:**

```javascript
// loadtest.js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '30s', target: 20 },
    { duration: '1m', target: 50 },
    { duration: '30s', target: 0 }
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],
    http_req_failed: ['rate<0.01']
  }
};

export default function () {
  const response = http.get('http://localhost:3000/api/users');
  
  check(response, {
    'status is 200': (r) => r.status === 200,
    'response time < 500ms': (r) => r.timings.duration < 500
  });
  
  sleep(1);
}
```

**Testing Strategy:**

- **Unit tests**: 70% - Test individual functions
- **Integration tests**: 20% - Test API endpoints
- **E2E tests**: 10% - Test critical user flows
- Aim for >80% code coverage
- Use TDD for complex logic
- Mock external dependencies
- Test error scenarios
- Implement CI/CD pipeline

### Q50: What deployment strategies do you use for Node.js applications?

**1. Docker containerization:**

```dockerfile
# Multi-stage build
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

# Production image
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force

COPY --from=builder /app/dist ./dist

ENV NODE_ENV=production
EXPOSE 3000

USER node

CMD ["node", "dist/index.js"]
```

**2. Kubernetes deployment:**

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 3000
        env:
        - name: NODE_ENV
          value: "production"
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
        resources:
          limits:
            memory: "512Mi"
            cpu: "500m"
          requests:
            memory: "256Mi"
            cpu: "250m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5

---
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 3000
  type: LoadBalancer
```

**3. PM2 ecosystem:**

```javascript
// ecosystem.config.js
module.exports = {
  apps: [{
    name: 'api',
    script: './dist/index.js',
    instances: 'max',
    exec_mode: 'cluster',
    max_memory_restart: '1G',
    env: {
      NODE_ENV: 'production',
      PORT: 3000
    },
    error_file: './logs/err.log',
    out_file: './logs/out.log',
    log_date_format: 'YYYY-MM-DD HH:mm Z',
    merge_logs: true,
    autorestart: true,
    watch: false,
    max_restarts: 10,
    min_uptime: '10s'
  }],
  
  deploy: {
    production: {
      user: 'node',
      host: 'server.example.com',
      ref: 'origin/main',
      repo: 'git@github.com:user/repo.git',
      path: '/var/www/myapp',
      'post-deploy': 'npm install && pm2 reload ecosystem.config.js --env production'
    }
  }
};
```

**4. CI/CD with GitHub Actions:**

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Run linter
        run: npm run lint

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Build Docker image
        run: docker build -t myapp:${{ github.sha }} .
      
      - name: Push to registry
        run: |
          echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
          docker tag myapp:${{ github.sha }} myapp:latest
          docker push myapp:latest

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Kubernetes
        run: |
          kubectl set image deployment/myapp myapp=myapp:${{ github.sha }}
          kubectl rollout status deployment/myapp
```

**Best Practices:**

- Use containerization (Docker)
- Implement blue-green deployment
- Use health checks
- Implement graceful shutdown
- Use environment-specific configs
- Implement monitoring and logging
- Use CDN for static assets
- Implement auto-scaling
- Use managed services (AWS ECS, Google Cloud Run)
