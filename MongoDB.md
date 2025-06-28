# MongoDB Notes

## Table of Contents
1. [Introduction](#introduction)
2. [MongoDB Atlas Setup](#mongodb-atlas-setup)
3. [Database Concepts](#database-concepts)
4. [CRUD Operations](#crud-operations)
5. [Data Types](#data-types)
6. [Querying Data](#querying-data)
7. [Indexing](#indexing)
8. [Aggregation Pipeline](#aggregation-pipeline)
9. [Data Modeling](#data-modeling)
10. [Mongoose ODM](#mongoose-odm)
11. [Atlas Security & Authentication](#atlas-security--authentication)
12. [Atlas Performance Optimization](#atlas-performance-optimization)
13. [Atlas Best Practices](#atlas-best-practices)

---

## Introduction

**MongoDB** is a NoSQL, document-oriented database that stores data in flexible, JSON-like documents. It's designed for scalability, performance, and ease of development.

### Key Features:
- **Document-Oriented**: Stores data in BSON (Binary JSON) format
- **Schema-less**: Flexible document structure
- **Scalable**: Horizontal scaling with sharding
- **High Performance**: Optimized for read and write operations
- **Rich Query Language**: Supports complex queries and aggregations
- **Indexing**: Comprehensive indexing support

### MongoDB vs SQL Databases:

| MongoDB | SQL Database |
|---------|--------------|
| Document | Row |
| Collection | Table |
| Field | Column |
| Embedded Document | Join |
| _id | Primary Key |

---

## MongoDB Atlas Setup

### Create MongoDB Atlas Account
1. Sign up at [mongodb.com/atlas](https://www.mongodb.com/atlas)
2. Create a free M0 cluster (512 MB storage, shared RAM)
3. Choose your cloud provider (AWS, Google Cloud, Azure)
4. Select a region close to your location

### Configure Database Access
```javascript
// Create database user
// Go to Database Access > Add New Database User
Username: myAppUser
Password: securePassword123
Database User Privileges: Read and write to any database
```

### Configure Network Access
```javascript
// Go to Network Access > Add IP Address
// For development: Add Current IP Address
// For production: Add specific IP addresses

// Allow access from anywhere (development only):
IP Address: 0.0.0.0/0
```

### Get Connection String
```javascript
// Go to Clusters > Connect > Connect your application
// Example connection strings:

// Standard connection string
mongodb+srv://myAppUser:securePassword123@cluster0.xxxxx.mongodb.net/myDatabase?retryWrites=true&w=majority

// With specific database
mongodb+srv://myAppUser:securePassword123@cluster0.xxxxx.mongodb.net/productionDB?retryWrites=true&w=majority
```

### Connecting to MongoDB Atlas

#### Using MongoDB Compass (GUI)
```bash
# Download MongoDB Compass from mongodb.com/products/compass
# Use the connection string from Atlas
mongodb+srv://myAppUser:securePassword123@cluster0.xxxxx.mongodb.net/
```

#### Using MongoDB Shell (mongosh)
```bash
# Install mongosh: npm install -g mongosh
mongosh "mongodb+srv://myAppUser:securePassword123@cluster0.xxxxx.mongodb.net/myDatabase"

# Or connect and then switch database
mongosh "mongodb+srv://myAppUser:securePassword123@cluster0.xxxxx.mongodb.net/"
use myDatabase
```

---

## Database Concepts

### MongoDB Atlas Structure
```
MongoDB Atlas Cluster
├── Database 1
│   ├── Collection 1
│   │   ├── Document 1
│   │   ├── Document 2
│   │   └── Document 3
│   └── Collection 2
└── Database 2
```

### Database Operations (MongoDB Atlas)
```javascript
// Connect to your Atlas cluster first
mongosh "mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/"

// Show all databases
show dbs

// Create/switch to database
use myDatabase

// Show current database
db

// Drop database (be careful!)
db.dropDatabase()

// Show collections
show collections

// Create collection
db.createCollection("users")

// Drop collection
db.users.drop()

// Database stats
db.stats()

// Collection stats
db.users.stats()

// Check connection status
db.runCommand({ ping: 1 })
```

---

## CRUD Operations

### Create Operations

#### Insert One Document
```javascript
db.users.insertOne({
  name: "John Doe",
  email: "john@example.com",
  age: 30,
  createdAt: new Date()
})
```

#### Insert Multiple Documents
```javascript
db.users.insertMany([
  {
    name: "Jane Smith",
    email: "jane@example.com",
    age: 25
  },
  {
    name: "Bob Johnson",
    email: "bob@example.com",
    age: 35
  }
])
```

### Read Operations

#### Find All Documents
```javascript
db.users.find()

// Pretty format
db.users.find().pretty()
```

#### Find with Query
```javascript
// Find by field
db.users.find({ age: 30 })

// Find with conditions
db.users.find({ age: { $gte: 25 } })

// Find one document
db.users.findOne({ email: "john@example.com" })
```

#### Projection (Select specific fields)
```javascript
// Include specific fields
db.users.find({}, { name: 1, email: 1 })

// Exclude specific fields
db.users.find({}, { password: 0 })
```

### Update Operations

#### Update One Document
```javascript
db.users.updateOne(
  { email: "john@example.com" },
  { $set: { age: 31, updatedAt: new Date() } }
)
```

#### Update Multiple Documents
```javascript
db.users.updateMany(
  { age: { $lt: 30 } },
  { $set: { category: "young" } }
)
```

#### Replace Document
```javascript
db.users.replaceOne(
  { email: "john@example.com" },
  {
    name: "John Smith",
    email: "john.smith@example.com",
    age: 32
  }
)
```

#### Upsert (Update or Insert)
```javascript
db.users.updateOne(
  { email: "new@example.com" },
  { $set: { name: "New User", age: 25 } },
  { upsert: true }
)
```

### Delete Operations

#### Delete One Document
```javascript
db.users.deleteOne({ email: "john@example.com" })
```

#### Delete Multiple Documents
```javascript
db.users.deleteMany({ age: { $lt: 18 } })
```

---

## Data Types

### MongoDB Data Types
```javascript
{
  // String
  name: "John Doe",
  
  // Number
  age: 30,
  price: 99.99,
  
  // Boolean
  isActive: true,
  
  // Date
  createdAt: new Date(),
  birthDate: ISODate("1990-01-01"),
  
  // Array
  tags: ["mongodb", "database", "nosql"],
  scores: [85, 90, 78],
  
  // Object/Embedded Document
  address: {
    street: "123 Main St",
    city: "New York",
    zipCode: "10001"
  },
  
  // ObjectId
  _id: ObjectId("507f1f77bcf86cd799439011"),
  
  // Null
  middleName: null,
  
  // Binary Data
  profileImage: BinData(0, "..."),
  
  // Regular Expression
  pattern: /^[a-z]+$/i,
  
  // JavaScript Code
  code: function() { return this.name; }
}
```

---

## Querying Data

### Comparison Operators
```javascript
// Equal to
db.users.find({ age: 30 })

// Not equal to
db.users.find({ age: { $ne: 30 } })

// Greater than
db.users.find({ age: { $gt: 25 } })

// Greater than or equal
db.users.find({ age: { $gte: 25 } })

// Less than
db.users.find({ age: { $lt: 40 } })

// Less than or equal
db.users.find({ age: { $lte: 40 } })

// In array
db.users.find({ age: { $in: [25, 30, 35] } })

// Not in array
db.users.find({ age: { $nin: [25, 30, 35] } })
```

### Logical Operators
```javascript
// AND (implicit)
db.users.find({ age: 30, name: "John" })

// AND (explicit)
db.users.find({
  $and: [
    { age: { $gte: 25 } },
    { age: { $lt: 40 } }
  ]
})

// OR
db.users.find({
  $or: [
    { age: { $lt: 25 } },
    { age: { $gt: 65 } }
  ]
})

// NOT
db.users.find({ age: { $not: { $eq: 30 } } })

// NOR
db.users.find({
  $nor: [
    { age: { $lt: 18 } },
    { status: "inactive" }
  ]
})
```

### Element Operators
```javascript
// Field exists
db.users.find({ email: { $exists: true } })

// Field doesn't exist
db.users.find({ middleName: { $exists: false } })

// Type check
db.users.find({ age: { $type: "number" } })
db.users.find({ age: { $type: 16 } }) // 16 = 32-bit integer
```

### Array Operators
```javascript
// All elements match
db.users.find({ tags: { $all: ["mongodb", "database"] } })

// Array size
db.users.find({ tags: { $size: 3 } })

// Element match
db.users.find({
  scores: {
    $elemMatch: { $gte: 80, $lt: 90 }
  }
})
```

### String Operators
```javascript
// Regular expression
db.users.find({ name: { $regex: /^John/i } })
db.users.find({ name: { $regex: "^John", $options: "i" } })

// Text search (requires text index)
db.users.find({ $text: { $search: "John Doe" } })
```

### Cursor Methods
```javascript
// Limit results
db.users.find().limit(5)

// Skip results
db.users.find().skip(10)

// Sort results
db.users.find().sort({ age: 1 })  // Ascending
db.users.find().sort({ age: -1 }) // Descending

// Count documents
db.users.find({ age: { $gte: 18 } }).count()

// Chain methods
db.users.find({ age: { $gte: 18 } })
  .sort({ name: 1 })
  .limit(10)
  .skip(5)
```

---

## Indexing

### Why Indexing?
- **Performance**: Faster query execution
- **Sorting**: Efficient sorting operations
- **Uniqueness**: Enforce unique constraints

### Index Types

#### Single Field Index
```javascript
// Create index
db.users.createIndex({ email: 1 })  // Ascending
db.users.createIndex({ age: -1 })   // Descending

// Unique index
db.users.createIndex({ email: 1 }, { unique: true })
```

#### Compound Index
```javascript
// Multiple fields
db.users.createIndex({ age: 1, name: 1 })

// Order matters for compound indexes
db.users.createIndex({ status: 1, createdAt: -1 })
```

#### Text Index
```javascript
// Single field text index
db.articles.createIndex({ title: "text" })

// Multiple field text index
db.articles.createIndex({
  title: "text",
  content: "text"
})

// Text search
db.articles.find({ $text: { $search: "mongodb tutorial" } })
```

#### Geospatial Index
```javascript
// 2dsphere index for geographic data
db.places.createIndex({ location: "2dsphere" })

// Find nearby locations
db.places.find({
  location: {
    $near: {
      $geometry: { type: "Point", coordinates: [-73.9857, 40.7484] },
      $maxDistance: 1000
    }
  }
})
```

### Index Management
```javascript
// List indexes
db.users.getIndexes()

// Drop index
db.users.dropIndex({ email: 1 })
db.users.dropIndex("email_1")

// Drop all indexes (except _id)
db.users.dropIndexes()

// Index statistics
db.users.stats()

// Explain query execution
db.users.find({ email: "john@example.com" }).explain("executionStats")
```

---

## Aggregation Pipeline

### Introduction
The aggregation pipeline is a framework for data aggregation modeled on the concept of data processing pipelines.

### Basic Pipeline Structure
```javascript
db.collection.aggregate([
  { $stage1: { ... } },
  { $stage2: { ... } },
  { $stage3: { ... } }
])
```

### Common Pipeline Stages

#### $match - Filter Documents
```javascript
db.orders.aggregate([
  { $match: { status: "completed" } }
])
```

#### $project - Select/Transform Fields
```javascript
db.users.aggregate([
  {
    $project: {
      name: 1,
      email: 1,
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      ageGroup: {
        $cond: {
          if: { $gte: ["$age", 18] },
          then: "adult",
          else: "minor"
        }
      }
    }
  }
])
```

#### $group - Group and Aggregate
```javascript
// Group by field and count
db.orders.aggregate([
  {
    $group: {
      _id: "$status",
      count: { $sum: 1 },
      totalAmount: { $sum: "$amount" },
      avgAmount: { $avg: "$amount" },
      maxAmount: { $max: "$amount" },
      minAmount: { $min: "$amount" }
    }
  }
])

// Group by multiple fields
db.sales.aggregate([
  {
    $group: {
      _id: {
        year: { $year: "$date" },
        month: { $month: "$date" }
      },
      totalSales: { $sum: "$amount" }
    }
  }
])
```

#### $sort - Sort Documents
```javascript
db.users.aggregate([
  { $sort: { age: -1, name: 1 } }
])
```

#### $limit and $skip
```javascript
db.users.aggregate([
  { $sort: { age: -1 } },
  { $skip: 10 },
  { $limit: 5 }
])
```

#### $unwind - Flatten Arrays
```javascript
// Original document: { name: "John", tags: ["a", "b", "c"] }
// After $unwind: 
// { name: "John", tags: "a" }
// { name: "John", tags: "b" }
// { name: "John", tags: "c" }

db.users.aggregate([
  { $unwind: "$tags" }
])
```

#### $lookup - Join Collections
```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "userInfo"
    }
  }
])
```

### Complex Aggregation Example
```javascript
db.orders.aggregate([
  // Stage 1: Filter completed orders from 2023
  {
    $match: {
      status: "completed",
      orderDate: {
        $gte: ISODate("2023-01-01"),
        $lt: ISODate("2024-01-01")
      }
    }
  },
  
  // Stage 2: Join with users collection
  {
    $lookup: {
      from: "users",
      localField: "userId",
      foreignField: "_id",
      as: "user"
    }
  },
  
  // Stage 3: Unwind user array
  { $unwind: "$user" },
  
  // Stage 4: Group by user and calculate metrics
  {
    $group: {
      _id: "$user._id",
      userName: { $first: "$user.name" },
      totalOrders: { $sum: 1 },
      totalAmount: { $sum: "$amount" },
      avgOrderValue: { $avg: "$amount" }
    }
  },
  
  // Stage 5: Sort by total amount
  { $sort: { totalAmount: -1 } },
  
  // Stage 6: Limit to top 10 customers
  { $limit: 10 }
])
```

---

## Data Modeling

### Embedded vs Referenced Documents

#### Embedded Documents (Denormalization)
```javascript
// User with embedded addresses
{
  _id: ObjectId("..."),
  name: "John Doe",
  email: "john@example.com",
  addresses: [
    {
      type: "home",
      street: "123 Main St",
      city: "New York",
      zipCode: "10001"
    },
    {
      type: "work",
      street: "456 Oak Ave",
      city: "Boston",
      zipCode: "02101"
    }
  ]
}
```

**Use When:**
- Related data is accessed together
- Data doesn't change frequently
- Document size won't exceed 16MB

#### Referenced Documents (Normalization)
```javascript
// Users collection
{
  _id: ObjectId("user1"),
  name: "John Doe",
  email: "john@example.com"
}

// Orders collection
{
  _id: ObjectId("order1"),
  userId: ObjectId("user1"),
  items: [...],
  total: 99.99
}
```

**Use When:**
- Data is accessed independently
- Data changes frequently
- Many-to-many relationships
- Document size concerns

### Schema Design Patterns

#### One-to-One (Embedded)
```javascript
// User profile embedded in user document
{
  _id: ObjectId("..."),
  username: "johndoe",
  email: "john@example.com",
  profile: {
    firstName: "John",
    lastName: "Doe",
    bio: "Software developer",
    avatar: "avatar.jpg"
  }
}
```

#### One-to-Many (Embedded Array)
```javascript
// Blog post with embedded comments
{
  _id: ObjectId("..."),
  title: "MongoDB Tutorial",
  content: "...",
  comments: [
    {
      author: "Alice",
      text: "Great tutorial!",
      date: ISODate("...")
    },
    {
      author: "Bob",
      text: "Very helpful",
      date: ISODate("...")
    }
  ]
}
```

#### One-to-Many (Referenced)
```javascript
// Category and Products
// Categories collection
{
  _id: ObjectId("cat1"),
  name: "Electronics",
  description: "Electronic devices"
}

// Products collection
{
  _id: ObjectId("prod1"),
  name: "Laptop",
  price: 999.99,
  categoryId: ObjectId("cat1")
}
```

#### Many-to-Many
```javascript
// Students and Courses
// Students collection
{
  _id: ObjectId("student1"),
  name: "John Doe",
  courseIds: [
    ObjectId("course1"),
    ObjectId("course2")
  ]
}

// Courses collection
{
  _id: ObjectId("course1"),
  name: "Database Design",
  studentIds: [
    ObjectId("student1"),
    ObjectId("student2")
  ]
}
```

---

## Mongoose ODM

### MongoDB Atlas Connection Setup
```bash
npm install mongoose dotenv
```

```javascript
// config/database.js
const mongoose = require('mongoose');

const connectDB = async () => {
  try {
    // MongoDB Atlas connection string
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    
    console.log(`MongoDB Connected: ${conn.connection.host}`);
    console.log(`Database: ${conn.connection.name}`);
  } catch (error) {
    console.error('Database connection error:', error);
    process.exit(1);
  }
};

module.exports = connectDB;
```

### Environment Variables (.env file)
```bash
# .env file
MONGODB_URI=mongodb+srv://myAppUser:securePassword123@cluster0.xxxxx.mongodb.net/myDatabase?retryWrites=true&w=majority
NODE_ENV=development
PORT=3000

# For different environments
MONGODB_URI_DEV=mongodb+srv://user:pass@cluster0.xxxxx.mongodb.net/devDB?retryWrites=true&w=majority
MONGODB_URI_PROD=mongodb+srv://user:pass@cluster0.xxxxx.mongodb.net/prodDB?retryWrites=true&w=majority
```

### App.js Setup
```javascript
// app.js
require('dotenv').config();
const express = require('express');
const connectDB = require('./config/database');

const app = express();

// Connect to MongoDB Atlas
connectDB();

// Middleware
app.use(express.json());

// Routes
app.get('/', (req, res) => {
  res.json({ 
    message: 'Connected to MongoDB Atlas',
    database: process.env.MONGODB_URI ? 'Atlas' : 'Local'
  });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Schema Definition
```javascript
// models/User.js
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    maxlength: [50, 'Name cannot be more than 50 characters']
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: [/^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/, 'Please add a valid email']
  },
  age: {
    type: Number,
    min: [0, 'Age must be positive'],
    max: [120, 'Age cannot exceed 120']
  },
  isActive: {
    type: Boolean,
    default: true
  },
  tags: [String],
  address: {
    street: String,
    city: String,
    zipCode: String
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
}, {
  timestamps: true // Adds createdAt and updatedAt automatically
});

// Virtual field
userSchema.virtual('fullInfo').get(function() {
  return `${this.name} (${this.email})`;
});

// Instance method
userSchema.methods.getPublicProfile = function() {
  return {
    name: this.name,
    email: this.email,
    isActive: this.isActive
  };
};

// Static method
userSchema.statics.findByEmail = function(email) {
  return this.findOne({ email });
};

// Pre-save middleware
userSchema.pre('save', async function(next) {
  if (this.isModified('email')) {
    this.email = this.email.toLowerCase();
  }
  next();
});

module.exports = mongoose.model('User', userSchema);
```

### CRUD Operations with Mongoose
```javascript
const User = require('./models/User');

// Create
const createUser = async () => {
  try {
    const user = new User({
      name: 'John Doe',
      email: 'john@example.com',
      age: 30
    });
    await user.save();
    console.log('User created:', user);
  } catch (error) {
    console.error('Error creating user:', error);
  }
};

// Read
const getUsers = async () => {
  try {
    const users = await User.find({ isActive: true })
      .select('name email age')
      .sort({ createdAt: -1 })
      .limit(10);
    console.log('Users:', users);
  } catch (error) {
    console.error('Error fetching users:', error);
  }
};

// Update
const updateUser = async (id, updateData) => {
  try {
    const user = await User.findByIdAndUpdate(
      id,
      updateData,
      { new: true, runValidators: true }
    );
    console.log('Updated user:', user);
  } catch (error) {
    console.error('Error updating user:', error);
  }
};

// Delete
const deleteUser = async (id) => {
  try {
    await User.findByIdAndDelete(id);
    console.log('User deleted');
  } catch (error) {
    console.error('Error deleting user:', error);
  }
};
```

### Mongoose Relationships
```javascript
// Reference relationship
const postSchema = new mongoose.Schema({
  title: String,
  content: String,
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  }
});

// Populate referenced data
const getPostsWithAuthors = async () => {
  const posts = await Post.find()
    .populate('author', 'name email')
    .exec();
  console.log(posts);
};

// Embedded relationship
const orderSchema = new mongoose.Schema({
  customer: {
    name: String,
    email: String
  },
  items: [{
    product: String,
    quantity: Number,
    price: Number
  }],
  total: Number
});
```

---

## Atlas Security & Authentication

### MongoDB Atlas Security Features

#### Connection String Security
```javascript
// Never hardcode credentials in your code
// BAD ❌
const mongoURI = "mongodb+srv://myuser:mypassword@cluster0.xxxxx.mongodb.net/mydb";

// GOOD ✅
const mongoURI = process.env.MONGODB_URI;

// Use environment variables for different environments
const getMongoURI = () => {
  if (process.env.NODE_ENV === 'production') {
    return process.env.MONGODB_URI_PROD;
  } else if (process.env.NODE_ENV === 'development') {
    return process.env.MONGODB_URI_DEV;
  }
  return process.env.MONGODB_URI;
};
```

#### Atlas Database Users Management
```javascript
// Atlas Console > Database Access
// Create different users for different purposes:

// Application user (read/write specific database)
{
  username: "appUser",
  password: "secureAppPassword",
  roles: [
    {
      role: "readWrite",
      db: "myApplicationDB"
    }
  ]
}

// Analytics user (read-only)
{
  username: "analyticsUser", 
  password: "analyticsPassword",
  roles: [
    {
      role: "read",
      db: "myApplicationDB"
    }
  ]
}

// Admin user (database admin)
{
  username: "dbAdmin",
  password: "adminPassword", 
  roles: [
    {
      role: "dbAdmin",
      db: "myApplicationDB"
    }
  ]
}
```

#### Network Security (IP Whitelist)
```javascript
// Atlas Console > Network Access
// Add IP addresses that can connect:

// Development (your current IP)
IP: 123.456.789.101/32

// Production server
IP: 203.456.789.102/32

// Office network
IP: 192.168.1.0/24

// Temporary access (expires in 6 hours)
IP: 0.0.0.0/0 (Delete after development)
```

#### Connection String Options for Security
```javascript
// Secure connection string with additional options
const secureURI = `mongodb+srv://${username}:${password}@${cluster}/${database}?retryWrites=true&w=majority&ssl=true&authSource=admin`;

// Connection with SSL and authentication
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  ssl: true,
  sslValidate: true,
  authSource: 'admin'
});
```

### Mongoose Security Best Practices
```javascript
// Password hashing
const bcrypt = require('bcryptjs');

userSchema.pre('save', async function(next) {
  if (!this.isModified('password')) return next();
  
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
  next();
});

// Input validation
const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    validate: {
      validator: function(v) {
        return /^\w+@[a-zA-Z_]+?\.[a-zA-Z]{2,3}$/.test(v);
      },
      message: 'Invalid email format'
    }
  }
});

// Query sanitization
const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());
```

---

## Atlas Performance Optimization

### Atlas Query Optimization
```javascript
// Use indexes for Atlas queries
db.users.createIndex({ email: 1 })

// Use projection to limit fields (saves bandwidth with Atlas)
db.users.find({}, { name: 1, email: 1 })

// Use limit and skip for pagination (important for cloud connections)
db.users.find().skip(20).limit(10)

// Explain query performance on Atlas
db.users.find({ email: "john@example.com" }).explain("executionStats")

// Check Atlas cluster performance
db.serverStatus()
```

### MongoDB Atlas Connection Optimization
```javascript
// Optimized connection for Atlas
mongoose.connect(process.env.MONGODB_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
  
  // Connection pool settings for Atlas
  maxPoolSize: 10,        // Maximum number of connections
  minPoolSize: 2,         // Minimum number of connections
  maxIdleTimeMS: 30000,   // Close connections after 30 seconds of inactivity
  
  // Timeouts for Atlas
  serverSelectionTimeoutMS: 5000,  // How long to try selecting a server
  socketTimeoutMS: 45000,          // How long to wait for a response
  connectTimeoutMS: 10000,         // How long to wait for initial connection
  
  // Atlas-specific optimizations
  retryWrites: true,      // Retry writes that fail due to network issues
  w: 'majority',          // Write concern for Atlas clusters
  
  // Buffer settings
  bufferCommands: true,   // Enable command buffering
  bufferMaxEntries: 0     // Disable buffering if connection is lost
});

// Connection event handlers for Atlas
mongoose.connection.on('connected', () => {
  console.log('✅ Connected to MongoDB Atlas');
});

mongoose.connection.on('error', (err) => {
  console.error('❌ MongoDB Atlas connection error:', err);
});

mongoose.connection.on('disconnected', () => {
  console.log('⚠️ Disconnected from MongoDB Atlas');
});

// Graceful shutdown for Atlas
process.on('SIGINT', async () => {
  await mongoose.connection.close();
  console.log('MongoDB Atlas connection closed through app termination');
  process.exit(0);
});
```

### Atlas Performance Monitoring
```javascript
// Enable monitoring in your app
const connectDB = async () => {
  try {
    mongoose.set('debug', process.env.NODE_ENV === 'development');
    
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      useNewUrlParser: true,
      useUnifiedTopology: true,
    });
    
    // Log connection details
    console.log(`MongoDB Atlas Connected: ${conn.connection.host}`);
    console.log(`Database: ${conn.connection.name}`);
    console.log(`Ready State: ${conn.connection.readyState}`);
    
  } catch (error) {
    console.error('Atlas connection error:', error);
    process.exit(1);
  }
};

// Monitor query performance
mongoose.connection.on('connected', () => {
  console.log('Atlas connection established');
});

// Atlas-specific error handling
mongoose.connection.on('error', (err) => {
  if (err.name === 'MongoNetworkError') {
    console.error('Network error connecting to Atlas:', err.message);
  } else if (err.name === 'MongoAuthenticationError') {
    console.error('Authentication failed for Atlas:', err.message);
  } else {
    console.error('Atlas error:', err);
  }
});
```

### Data Modeling for Performance
```javascript
// Avoid large arrays
// Instead of this:
{
  postId: ObjectId("..."),
  comments: [/* thousands of comments */]
}

// Use this:
// Posts collection
{ _id: ObjectId("post1"), title: "..." }

// Comments collection
{ _id: ObjectId("..."), postId: ObjectId("post1"), text: "..." }
```

---

## Atlas Best Practices

### 1. Schema Design
- Embed data that is accessed together
- Reference data that is accessed independently
- Avoid deep nesting (max 3-4 levels)
- Keep document size under 16MB
- Use appropriate data types

### 2. Indexing
- Create indexes for frequently queried fields
- Use compound indexes for multi-field queries
- Monitor index usage with db.collection.stats()
- Remove unused indexes
- Use partial indexes for sparse data

### 3. Query Optimization
- Use projection to limit returned fields
- Use limit() and skip() for pagination
- Avoid regex queries on large collections
- Use aggregation pipeline for complex queries
- Profile slow queries with explain()

### 4. Security
- Enable authentication and authorization
- Use strong passwords
- Limit network exposure
- Validate input data
- Use encrypted connections (TLS/SSL)
- Regular security updates

### 5. Mongoose Best Practices
```javascript
// Use lean() for read-only queries
const users = await User.find().lean();

// Select only needed fields
const users = await User.find().select('name email');

// Use proper error handling
try {
  const user = await User.findById(id);
  if (!user) {
    throw new Error('User not found');
  }
} catch (error) {
  console.error('Database error:', error);
}

// Use transactions for related operations
const session = await mongoose.startSession();
try {
  session.startTransaction();
  
  await User.create([userData], { session });
  await Profile.create([profileData], { session });
  
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
  throw error;
} finally {
  session.endSession();
}
```

### 6. Monitoring and Maintenance
- Monitor database performance metrics
- Set up proper logging
- Regular backups
- Monitor storage usage
- Plan for scaling (sharding, replica sets)

---

## MongoDB Atlas Specific Best Practices

### 1. Connection String Management
```javascript
// Environment-based configuration
const config = {
  development: {
    MONGODB_URI: process.env.MONGODB_URI_DEV,
    options: { maxPoolSize: 5 }
  },
  production: {
    MONGODB_URI: process.env.MONGODB_URI_PROD,
    options: { maxPoolSize: 20 }
  },
  test: {
    MONGODB_URI: process.env.MONGODB_URI_TEST,
    options: { maxPoolSize: 2 }
  }
};

const currentConfig = config[process.env.NODE_ENV || 'development'];
```

### 2. Atlas Cluster Optimization
```javascript
// Use read preferences for better performance
const readPreference = {
  // Primary: Default, for write-heavy operations
  primary: 'primary',
  
  // Secondary: For read-heavy analytics
  secondary: 'secondary', 
  
  // Nearest: For geographically distributed apps
  nearest: 'nearest'
};

// Apply read preference in queries
const users = await User.find().read('secondary');
```

### 3. Atlas Monitoring and Alerts
```javascript
// Connection health check endpoint
app.get('/health', async (req, res) => {
  try {
    const dbState = mongoose.connection.readyState;
    const states = {
      0: 'disconnected',
      1: 'connected', 
      2: 'connecting',
      3: 'disconnecting'
    };
    
    if (dbState === 1) {
      // Test database with a simple query
      await mongoose.connection.db.admin().ping();
      res.status(200).json({
        status: 'healthy',
        database: states[dbState],
        timestamp: new Date().toISOString()
      });
    } else {
      res.status(503).json({
        status: 'unhealthy',
        database: states[dbState],
        timestamp: new Date().toISOString()
      });
    }
  } catch (error) {
    res.status(503).json({
      status: 'unhealthy',
      error: error.message,
      timestamp: new Date().toISOString()
    });
  }
});
```

### 4. Atlas Data Migration
```javascript
// Migrate data between clusters
const migrateData = async () => {
  const sourceURI = process.env.SOURCE_MONGODB_URI;
  const targetURI = process.env.TARGET_MONGODB_URI;
  
  const sourceConnection = await mongoose.createConnection(sourceURI);
  const targetConnection = await mongoose.createConnection(targetURI);
  
  try {
    const collections = await sourceConnection.db.listCollections().toArray();
    
    for (const collection of collections) {
      const collectionName = collection.name;
      const data = await sourceConnection.db.collection(collectionName).find().toArray();
      
      if (data.length > 0) {
        await targetConnection.db.collection(collectionName).insertMany(data);
        console.log(`Migrated ${data.length} documents from ${collectionName}`);
      }
    }
  } finally {
    await sourceConnection.close();
    await targetConnection.close();
  }
};
```

### 5. Atlas Backup Strategy
```javascript
// Automated backup reminder function
const scheduleBackup = () => {
  // Atlas provides automatic backups, but you can create custom exports
  const createBackup = async () => {
    try {
      const collections = await mongoose.connection.db.listCollections().toArray();
      const backup = {};
      
      for (const collection of collections) {
        const collectionName = collection.name;
        backup[collectionName] = await mongoose.connection.db
          .collection(collectionName)
          .find()
          .toArray();
      }
      
      // Save backup to file or cloud storage
      const fs = require('fs');
      const backupData = JSON.stringify(backup, null, 2);
      const filename = `backup_${new Date().toISOString().split('T')[0]}.json`;
      
      fs.writeFileSync(filename, backupData);
      console.log(`Backup created: ${filename}`);
      
    } catch (error) {
      console.error('Backup failed:', error);
    }
  };
  
  // Schedule backup (using node-cron or similar)
  // cron.schedule('0 2 * * *', createBackup); // Daily at 2 AM
};
```

### 6. Atlas-Specific Error Handling
```javascript
const handleAtlasErrors = (error) => {
  if (error.name === 'MongoNetworkError') {
    return {
      status: 503,
      message: 'Database temporarily unavailable. Please try again.',
      code: 'NETWORK_ERROR'
    };
  }
  
  if (error.name === 'MongoServerSelectionError') {
    return {
      status: 503,
      message: 'Unable to connect to database cluster.',
      code: 'SERVER_SELECTION_ERROR'
    };
  }
  
  if (error.name === 'MongoAuthenticationError') {
    return {
      status: 401,
      message: 'Database authentication failed.',
      code: 'AUTH_ERROR'
    };
  }
  
  if (error.code === 11000) {
    return {
      status: 409,
      message: 'Duplicate key error.',
      code: 'DUPLICATE_KEY'
    };
  }
  
  return {
    status: 500,
    message: 'Internal server error.',
    code: 'UNKNOWN_ERROR'
  };
};

// Global error handler middleware
app.use((error, req, res, next) => {
  const handledError = handleAtlasErrors(error);
  res.status(handledError.status).json(handledError);
});
```

### 7. Development vs Production Atlas Setup
```javascript
// config/database.js
const isDevelopment = process.env.NODE_ENV === 'development';
const isProduction = process.env.NODE_ENV === 'production';

const getConnectionOptions = () => {
  const baseOptions = {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  };
  
  if (isProduction) {
    return {
      ...baseOptions,
      maxPoolSize: 20,
      minPoolSize: 5,
      maxIdleTimeMS: 30000,
      serverSelectionTimeoutMS: 10000,
      socketTimeoutMS: 45000,
      retryWrites: true,
      w: 'majority'
    };
  }
  
  if (isDevelopment) {
    return {
      ...baseOptions,
      maxPoolSize: 5,
      minPoolSize: 1,
      maxIdleTimeMS: 10000,
      serverSelectionTimeoutMS: 5000,
      socketTimeoutMS: 30000
    };
  }
  
  return baseOptions;
};

const connectDB = async () => {
  try {
    const options = getConnectionOptions();
    await mongoose.connect(process.env.MONGODB_URI, options);
    
    if (isDevelopment) {
      mongoose.set('debug', true);
    }
    
    console.log(`MongoDB Atlas connected in ${process.env.NODE_ENV} mode`);
  } catch (error) {
    console.error('Atlas connection failed:', error);
    process.exit(1);
  }
};
```

### 8. Atlas Data Validation
```javascript
// Enhanced schema validation for Atlas
const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true,
    validate: {
      validator: async function(email) {
        // Only check uniqueness if email is modified
        if (!this.isModified('email')) return true;
        
        const user = await this.constructor.findOne({ email });
        return !user;
      },
      message: 'Email already exists'
    }
  },
  password: {
    type: String,
    required: true,
    minlength: 8,
    validate: {
      validator: function(password) {
        // Strong password validation
        return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/.test(password);
      },
      message: 'Password must contain uppercase, lowercase, number and special character'
    }
  }
}, {
  timestamps: true,
  // Atlas-optimized settings
  collection: 'users', // Explicit collection name
  versionKey: false     // Remove __v field
});

// Compound index for Atlas optimization
userSchema.index({ email: 1, createdAt: -1 });
userSchema.index({ 'profile.firstName': 'text', 'profile.lastName': 'text' });
```

---

## Atlas-Specific Commands Reference

### Atlas Connection Commands
```javascript
// Connect to Atlas cluster
mongosh "mongodb+srv://username:password@cluster0.xxxxx.mongodb.net/database"

// Check Atlas connection status
db.runCommand({ ping: 1 })
