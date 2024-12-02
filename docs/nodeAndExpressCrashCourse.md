# 📚 **Node.js & Express.js Crash Course Documentation**

Welcome to the **Node.js & Express.js Crash Course**! In this guide, you'll learn how to build a backend server using **Node.js** and **Express.js**, covering fundamental concepts like middleware, file uploads, authentication (JWT with Refresh Token), database integration (MongoDB and PostgreSQL), and more. By the end, you’ll be ready to create real-world web applications and APIs.

---

## 🚀 **Getting Started with Node.js & Express.js**

### **Step 1: Install Node.js**

Download and install **Node.js** from [nodejs.org](https://nodejs.org/), which includes **npm** (Node Package Manager) for managing dependencies.

### **Step 2: Set Up a Node.js Project**

Create a new project directory, initialize a Node.js project, and install necessary dependencies.

```bash
mkdir my-express-app
cd my-express-app
npm init -y
npm install express morgan multer jsonwebtoken bcryptjs mongoose pg
npm install --save-dev nodemon
```

This installs:
- **express**: Core framework for building APIs.
- **morgan**: Middleware for logging HTTP requests.
- **multer**: Middleware for handling file uploads.
- **jsonwebtoken**: JWT authentication package.
- **bcryptjs**: For hashing passwords.
- **mongoose**: MongoDB ORM for managing database operations.
- **pg**: PostgreSQL client for Node.js.
- **nodemon**: For auto-restarting the server during development.

---

## 🔨 **Building Your First Express Server**

### **Basic Express Setup**

Create **`server.js`** to set up a basic Express server.

```js
const express = require('express');
const morgan = require('morgan');
const app = express();
const port = 3000;

// Use Morgan for logging HTTP requests
app.use(morgan('dev'));

// Middleware to parse JSON request bodies
app.use(express.json());

// Basic route
app.get('/', (req, res) => {
  res.send('Hello, World!');
});

// Start server
app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### **Running the Server with Nodemon**

Update your **`package.json`** to include a start script:

```json
"scripts": {
  "start": "nodemon server.js"
}
```

Run the server with:

```bash
npm start
```

---

## 🔧 **Middleware in Express.js**

Middleware functions are essential in Express for tasks like logging, authentication, and handling file uploads. Here’s how to use **Morgan** and **Multer**.

### **Using Morgan for Request Logging**

**Morgan** logs HTTP requests, helping with debugging and monitoring.

```js
const morgan = require('morgan');
app.use(morgan('dev'));
```

### **Handling File Uploads with Multer**

**Multer** handles file uploads. Here’s how you can use it:

```js
const multer = require('multer');

// Configure Multer storage options
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + '-' + file.originalname);
  }
});

// Initialize Multer
const upload = multer({ storage });

// File upload route
app.post('/upload', upload.single('file'), (req, res) => {
  if (!req.file) return res.status(400).send('No file uploaded.');
  res.send(`File uploaded: ${req.file.filename}`);
});
```

---

## 🔒 **Authentication: JWT with Refresh Tokens**

Using **JWT** (JSON Web Token) for authentication is common in modern web apps. One challenge with JWT is token expiration. To handle this, you can implement **Refresh Tokens**.

### **JWT Authentication with Refresh Tokens**

1. **Install JWT and BcryptJS**:

```bash
npm install jsonwebtoken bcryptjs
```

2. **JWT and Refresh Token Setup**:

Here’s how to issue an access token and a refresh token:

```js
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

// Dummy users database (in-memory)
const users = [
  { id: 1, username: 'alice', password: '$2a$10$EJ8uWg4P5o8VznMoc0e8t2k9VOVxY8xhFj4PfTgz9gqkxXy35S0aG' }, // bcrypt hash of 'password123'
];

// Middleware to verify JWT token
function verifyToken(req, res, next) {
  const token = req.header('Authorization')?.split(' ')[1]; // Bearer <token>
  if (!token) return res.status(401).send('Access Denied');

  jwt.verify(token, 'your_jwt_secret_key', (err, user) => {
    if (err) return res.status(403).send('Invalid Token');
    req.user = user;
    next();
  });
}

// Function to generate access and refresh tokens
function generateTokens(user) {
  const accessToken = jwt.sign({ id: user.id, username: user.username }, 'your_jwt_secret_key', { expiresIn: '15m' });
  const refreshToken = jwt.sign({ id: user.id }, 'your_refresh_token_secret', { expiresIn: '7d' });
  return { accessToken, refreshToken };
}

// Route to login and generate JWT + Refresh token
app.post('/login', (req, res) => {
  const { username, password } = req.body;

  const user = users.find((u) => u.username === username);
  if (!user) return res.status(400).send('User not found.');

  // Compare password with hash stored in database
  bcrypt.compare(password, user.password, (err, result) => {
    if (!result) return res.status(400).send('Invalid password.');

    // Generate access token and refresh token
    const { accessToken, refreshToken } = generateTokens(user);
    res.json({ accessToken, refreshToken });
  });
});

// Route to refresh the access token using the refresh token
app.post('/token', (req, res) => {
  const { refreshToken } = req.body;
  
  if (!refreshToken) return res.status(401).send('Refresh token is required.');

  jwt.verify(refreshToken, 'your_refresh_token_secret', (err, user) => {
    if (err) return res.status(403).send('Invalid refresh token.');

    // Generate new access token
    const { accessToken } = generateTokens(user);
    res.json({ accessToken });
  });
});

// Protected route using JWT
app.get('/protected', verifyToken, (req, res) => {
  res.send(`Hello ${req.user.username}, welcome to the protected route.`);
});
```

### **Explanation of Tokens:**

- **Access Token**: A short-lived token (usually expires in 15 minutes to 1 hour). Used to authenticate requests.
- **Refresh Token**: A long-lived token (usually expires in 7 days or longer). Used to obtain a new access token after it expires.

### **How It Works**:

1. The user logs in and receives an **access token** (short-lived) and a **refresh token** (long-lived).
2. The access token is used to authenticate API requests. It expires quickly (e.g., in 15 minutes).
3. If the access token expires, the client can send the refresh token to the server to obtain a new access token without requiring the user to log in again.

### **Testing**:

1. **Login**: Use the `/login` route to get the **access token** and **refresh token**.
2. **Refresh Access Token**: Use the `/token` route to refresh the **access token** using the refresh token.
3. **Access Protected Route**: Use the **access token** to access the `/protected` route.

---

## 🗄️ **Database Integration: MongoDB and PostgreSQL**

### **1. Integrating MongoDB with Mongoose**

1. **Install Mongoose**:

```bash
npm install mongoose
```

2. **Connect to MongoDB and Create a Model**:

```js
const mongoose = require('mongoose');

// MongoDB connection
mongoose.connect('mongodb://localhost/mydb', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB Connected'))
  .catch(err => console.log(err));

// Define a schema for users
const userSchema = new mongoose.Schema({
  username: { type: String, required: true },
  password: { type: String, required: true },
});

const User = mongoose.model('User', userSchema);

// Create a new user
app.post('/register', (req, res) => {
  const { username, password } = req.body;

  const newUser = new User({ username, password });
  newUser.save()
    .then(() => res.status(201).send('User registered'))
    .catch(err => res.status(400).send('Error registering user'));
});
```

### **2. Integrating PostgreSQL with pg**

1. **Install pg**:

```bash
npm install pg
```

2. **Connect to PostgreSQL and Query the Database**:

```js
const { Client } = require('pg');

// Connect to PostgreSQL
const client = new Client({
  user: 'myuser',
  host: 'localhost',
  database: 'mydb',
  password: 'mypassword',
  port: 5432,
});

client.connect();

// Query example
app.get('/users', async

 (req, res) => {
  const result = await client.query('SELECT * FROM users');
  res.json(result.rows);
});
```

---

## 📁 **Folder Structure Example**

Here’s an updated folder structure with MongoDB and PostgreSQL integrations:

```
/my-express-app
  /uploads                # Folder for file uploads
  /node_modules
  /public                 # Static files like HTML, CSS, JavaScript
  /src
    /controllers          # Controllers for handling logic
      userController.js
    /models               # MongoDB models or PostgreSQL queries
      userModel.js
    /routes               # API routes
      userRoutes.js
    /middlewares          # Custom middleware (e.g., JWT verification)
    server.js             # Main entry point
  /package.json           # Dependencies and scripts
  /tsconfig.json (if using TypeScript)
```

---

## 🚀 **Conclusion**

In this **Node.js & Express.js Crash Course**, you’ve learned how to build a RESTful API with authentication (JWT and Refresh Tokens), handle file uploads, integrate MongoDB and PostgreSQL databases, and manage complex routes. You now have the foundation to build scalable and secure backend applications.

### **Next Steps:**
- Add data validation and error handling.
- Implement advanced authentication (e.g., refresh tokens).
- Explore different database setups like **MySQL** or **Redis** for caching.

Happy coding! 🚀