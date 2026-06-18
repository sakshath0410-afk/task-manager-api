# Task Manager Backend

## Project Overview

Task Manager Backend is a RESTful API developed using Node.js, Express.js, and MongoDB. The backend provides task management functionalities and secure JWT-based authentication.

---

# Technologies Used

* Node.js
* Express.js
* MongoDB
* Mongoose
* JWT (jsonwebtoken)
* bcryptjs
* dotenv
* cors
* nodemon

---

# Project Structure

task-manager-api/
│
├── config/
│   └── db.js
│
├── controller/
│   ├── taskController.js
│   └── authController.js
│
├── models/
│   ├── Task.js
│   └── User.js
│
├── routes/
│   ├── taskRoutes.js
│   └── authRoutes.js
│
├── .env
├── server.js
├── package.json
└── package-lock.json

---

# Task 1: REST API Development (CRUD Operations)

## Objective

Develop REST APIs for task management using Express.js and MongoDB.

## Features

* Create Task
* Read Tasks
* Update Task
* Delete Task
* MongoDB Integration

---

## Task Model

### models/Task.js

```javascript id="7p0m4h"
import mongoose from "mongoose";

const taskSchema = new mongoose.Schema({
  title: String,
  description: String,
  status: {
    type: String,
    default: "Pending",
  },
});

export default mongoose.model("Task", taskSchema);
```

---

## API Endpoints

### Create Task

POST

```http id="2xv5hf"
/api/tasks
```

Request:

```json id="4l6r6o"
{
  "title": "Internship Task",
  "description": "Complete Task Manager",
  "status": "Pending"
}
```

---

### Get All Tasks

GET

```http id="fqu3n2"
/api/tasks
```

---

### Update Task

PUT

```http id="7r5x2o"
/api/tasks/:id
```

Request:

```json id="w9v8c6"
{
  "status": "Completed"
}
```

---

### Delete Task

DELETE

```http id="e7p8qf"
/api/tasks/:id
```

---

## Controller Example

```javascript id="q3q4ut"
export const getTasks = async (req, res) => {
  const tasks = await Task.find();
  res.json(tasks);
};
```

---

## Screenshot

Screenshot 1: Postman Create Task API

<img width="1414" height="764" alt="Screenshot 2026-06-18 111129" src="https://github.com/user-attachments/assets/a036474e-f0b3-41f8-9354-7a64444e2142" />


Screenshot 2: Postman Get Tasks API

<img width="1420" height="763" alt="Screenshot 2026-06-18 111341" src="https://github.com/user-attachments/assets/ae3769e6-339b-417f-9e91-c4828e7d6d31" />

Screenshot 3: Postman Update Task API

<img width="1414" height="452" alt="Screenshot 2026-06-18 111512" src="https://github.com/user-attachments/assets/374c54ef-22eb-422f-9bb7-c1f33522ec99" />


Screenshot 4: Postman Delete Task API

<img width="1422" height="439" alt="Screenshot 2026-06-18 111600" src="https://github.com/user-attachments/assets/eecd60c0-4aed-49c4-9104-efd986a38cd8" />

Screenshot 5: MongoDB Tasks Collection

<img width="651" height="867" alt="Screenshot 2026-06-18 111746" src="https://github.com/user-attachments/assets/f3f493a5-2d1d-4517-9224-2b913c56fa99" />

---

# Task 2: Database Integration & API Testing

## Objective

Connect backend APIs with MongoDB database and verify CRUD operations.

---

## MongoDB Connection

### config/db.js

```javascript id="n91g9w"
import mongoose from "mongoose";

const connectDB = async () => {
  try {
    await mongoose.connect(
      process.env.MONGO_URI
    );

    console.log("MongoDB Connected");
  } catch (error) {
    console.log(error);
  }
};

export default connectDB;
```

---

## Environment Variables

### .env

```env id="i9i2wn"
PORT=5000

MONGO_URI=mongodb://127.0.0.1:27017/taskdb
```

---

## Server Configuration

### server.js

```javascript id="gbm6mw"
import express from "express";
import dotenv from "dotenv";
import cors from "cors";

dotenv.config();

const app = express();

app.use(cors());
app.use(express.json());
```

---

## API Testing Tools

* Postman
* MongoDB Compass

---

## Screenshot

Screenshot 6: MongoDB Compass Connection

<img width="905" height="917" alt="Screenshot 2026-06-18 112029" src="https://github.com/user-attachments/assets/13d8ecc9-c76f-4012-a31b-93c28ad2761a" />


Screenshot 7: MongoDB Database

<img width="677" height="215" alt="Screenshot 2026-06-18 112051" src="https://github.com/user-attachments/assets/60c52e47-b376-43d5-8943-015683942442" />

Screenshot 8: Backend Running in Terminal

<img width="1404" height="434" alt="Screenshot 2026-06-18 112122" src="https://github.com/user-attachments/assets/2648e46c-0cc8-4a00-aa8a-4187e630c1c1" />


Example:

```bash id="v90lqv"
Server running on port 5000
MongoDB Connected
```

---

# Task 3: JWT Authentication & Authorization

## Objective

Implement secure authentication using JWT.

---

## Features

* User Registration
* User Login
* Password Hashing
* JWT Generation
* Protected Routes
* Logout Support

---

## User Model

### models/User.js

```javascript id="0ht0uw"
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  name: String,
  email: {
    type: String,
    unique: true,
  },
  password: String,
});

export default mongoose.model(
  "User",
  userSchema
);
```

---

## User Registration

### POST

```http id="j0r1zn"
/api/auth/register
```

Request:

```json id="l6wqqs"
{
  "name": "Navijith",
  "email": "navijith@gmail.com",
  "password": "123456"
}
```

---

## Password Hashing

```javascript id="bnrmvx"
const hashedPassword =
  await bcrypt.hash(password, 10);
```

---

## Login API

### POST

```http id="wj9wr8"
/api/auth/login
```

Request:

```json id="3m81q3"
{
  "email": "navijith@gmail.com",
  "password": "123456"
}
```

Response:

```json id="cjlwmf"
{
  "token":
  "eyJhbGciOiJIUzI1NiIs..."
}
```

---

## JWT Token Generation

```javascript id="h4rwvn"
const token = jwt.sign(
  { id: user._id },
  process.env.JWT_SECRET,
  { expiresIn: "1d" }
);
```

---

## Authentication Routes

### routes/authRoutes.js

```javascript id="z5q8cf"
router.post(
  "/register",
  registerUser
);

router.post(
  "/login",
  loginUser
);
```

---

## JWT Secret

### .env

```env id="4izjjc"
JWT_SECRET=mysecretkey
```

---

## Screenshot

Screenshot 9: Registration API Success

<img width="1419" height="749" alt="Screenshot 2026-06-18 112648" src="https://github.com/user-attachments/assets/46fecba0-9c3d-40d2-af5c-37a2b6d62828" />


Screenshot 10: Login API Success

<img width="1404" height="676" alt="Screenshot 2026-06-18 112716" src="https://github.com/user-attachments/assets/6d86be6a-56d4-48e6-a05c-29c564f3e2dd" />


Screenshot 11: JWT Token Generated

<img width="1238" height="249" alt="Screenshot 2026-06-18 112736" src="https://github.com/user-attachments/assets/0b430468-6b7a-4937-b6fa-306571db0066" />


Screenshot 12: MongoDB Users Collection

<img width="677" height="215" alt="Screenshot 2026-06-18 112051" src="https://github.com/user-attachments/assets/842fc752-405d-41f7-a3dc-ac34c7693638" />


---

# API Route Summary

## Task Routes

| Method | Endpoint       | Description |
| ------ | -------------- | ----------- |
| POST   | /api/tasks     | Create Task |
| GET    | /api/tasks     | Get Tasks   |
| PUT    | /api/tasks/:id | Update Task |
| DELETE | /api/tasks/:id | Delete Task |

---

## Authentication Routes

| Method | Endpoint           | Description   |
| ------ | ------------------ | ------------- |
| POST   | /api/auth/register | Register User |
| POST   | /api/auth/login    | Login User    |

---

# Installation Steps

## Clone Repository

```bash id="vw4m4m"
git clone https://github.com/Navijith/task-manager-api.git
```

---

## Navigate to Project

```bash id="f2r0ps"
cd task-manager-api
```

---

## Install Dependencies

```bash id="sgm5vo"
npm install
```

---

## Install Additional Packages

```bash id="vkh9zj"
npm install bcryptjs jsonwebtoken
```

---

## Run Server

```bash id="tb50nr"
npm run dev
```

Expected Output:

```bash id="jlwm7f"
Server running on port 5000
MongoDB Connected
```

---

# Commands Used

## Git Commands

```bash id="9bm2ma"
git init

git add .

git commit -m "Task completed"

git branch -M main

git remote add origin https://github.com/Navijith/task-manager-api.git

git pull origin main --rebase

git push origin main
```

---

# GitHub Repository Links

Backend Repository:

https://github.com/Navijith/task-manager-api

Frontend Repository:

https://github.com/Navijith/task-manager-frontend

---

# Security Features

* bcrypt Password Hashing
* JWT Authentication
* Environment Variables (.env)
* MongoDB Data Persistence
* Protected Routes Support
* Secure User Verification

---

# Outcome

Successfully developed a secure backend system with:

* RESTful CRUD APIs
* MongoDB Database Integration
* JWT Authentication
* Password Hashing
* User Registration & Login
* API Testing using Postman
* GitHub Version Control

The backend provides a secure and scalable foundation for the Task Manager application.
