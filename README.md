# Mongoose & MongoDB Guide 📌

A comprehensive guide to **Mongoose & MongoDB**, covering **MongoDB Compass**, **MongoDB Shell**, and essential commands. Learn how to set up a MongoDB database, use Mongoose for schema modeling, and interact with MongoDB via Compass and Shell with CRUD operations.

## 📌 Table of Contents
- [MongoDB Installation](#-mongodb-installation)
- [MongoDB Compass Guide](#-mongodb-compass-guide)
- [MongoDB Shell Commands](#-mongodb-shell-commands)
- [Mongoose Integration](#-mongoose-integration)
- [CRUD Operations](#-crud-operations)
- [Authentication & Security](#-authentication--security)

---

## 🚀 MongoDB Installation

### **Windows**
1. Download **MongoDB Community Edition** from [MongoDB Official Website](https://www.mongodb.com/try/download/community).
2. Install MongoDB and choose the option **MongoDB Compass** (GUI for MongoDB).
3. Start the MongoDB service using:  
   ```sh
   mongod
   ```

### **Linux/Mac**
1. Install MongoDB using Homebrew:
   ```sh
   brew tap mongodb/brew
   brew install mongodb-community@6.0
   ```
2. Start the MongoDB service:
   ```sh
   brew services start mongodb-community@6.0
   ```

---

## 🖥️ MongoDB Compass Guide

### **Connecting to MongoDB**
1. Open **MongoDB Compass**.
2. Click on **"Connect"** and enter the MongoDB URI:  
   ```
   mongodb://localhost:27017
   ```
3. Click **"Connect"** to access your MongoDB collections.

### **Creating a Database**
1. Click **"Create Database"**.
2. Enter a **Database Name** and **Collection Name**.
3. Click **"Create"**.

### **Performing CRUD Operations in Compass**
- **Insert Data:** Click on **"Insert Document"**, add JSON data, and save.
- **Edit Data:** Click on a document, modify fields, and save.
- **Delete Data:** Click on **"Delete Document"**.

---

## 📜 MongoDB Shell Commands

### **Basic Commands**
```sh
mongo  # Open MongoDB shell
show dbs  # List all databases
use myDatabase  # Switch to a database
db.createCollection("users")  # Create a collection
show collections  # Show all collections
```

### **CRUD Operations**
#### **Insert a Document**
```sh
db.users.insertOne({ name: "John Doe", email: "john@example.com" })
db.users.insertMany([{ name: "Alice" }, { name: "Bob" }])
```

#### **Read Documents**
```sh
db.users.find()  # Find all documents
db.users.findOne({ name: "John Doe" })  # Find one document
```

#### **Update a Document**
```sh
db.users.updateOne({ name: "John Doe" }, { $set: { email: "john.doe@example.com" } })
```

#### **Delete a Document**
```sh
db.users.deleteOne({ name: "John Doe" })
```

---

## ⚡ Mongoose Integration

### **Install Mongoose**
```sh
npm install mongoose
```

### **Connect to MongoDB**
```javascript
const mongoose = require("mongoose");

mongoose.connect("mongodb://localhost:27017/myDatabase", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
}).then(() => console.log("MongoDB Connected"))
  .catch(err => console.error(err));
```

### **Create a Mongoose Schema**
```javascript
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  password: String,
});

const User = mongoose.model("User", userSchema);
```

---

## 🛠️ CRUD Operations with Mongoose

### **Create a User**
```javascript
const newUser = new User({ name: "John", email: "john@example.com", password: "12345" });
await newUser.save();
```

### **Read Users**
```javascript
const users = await User.find();
console.log(users);
```

### **Update a User**
```javascript
await User.updateOne({ name: "John" }, { $set: { email: "newemail@example.com" } });
```

### **Delete a User**
```javascript
await User.deleteOne({ name: "John" });
```

---

## 🔐 Authentication & Security
- Use **bcrypt** for password hashing:
  ```sh
  npm install bcrypt
  ```
  ```javascript
  const bcrypt = require("bcrypt");
  const hashedPassword = await bcrypt.hash("myPassword", 10);
  ```
- Use **JWT** for authentication:
  ```sh
  npm install jsonwebtoken
  ```
  ```javascript
  const jwt = require("jsonwebtoken");
  const token = jwt.sign({ userId: user._id }, "secretkey", { expiresIn: "1h" });
  ```

---

## 🏗️ Tech Stack
- **Database:** MongoDB
- **ODM:** Mongoose
- **Backend:** Node.js, Express.js
- **Security:** JWT, bcrypt

## 🤝 Contributing
Feel free to submit **issues** and **pull requests**! Contributions are always welcome.  

## 📜 License
This project is open-source and available under the **MIT License**.

---

🚀 **Happy Learning!** 🎯
