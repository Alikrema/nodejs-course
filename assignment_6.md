---
layout: page
title: Session 11
parent: Assignments
---

# Homework Assignment: Express Validator, Authentication, and JWT

## Objective
In this assignment, you will create a simple backend application using Node.js, Express, Mongoose, and MongoDB. You'll implement user registration and login functionality with proper validation, authentication, and authorization using JWT.

## Requirements

### 1. Setup
- Initialize a new Node.js project
- Install necessary dependencies: express, mongoose, express-validator, bcrypt, jsonwebtoken, cookie-parser
- Set up a basic Express server and connect it to a MongoDB database using Mongoose

### 2. User Model
- Create a User model with the following fields:
  - username (String, required, unique)
  - email (String, required, unique)
  - password (String, required)
  - phone (String, optional)

### 3. User Registration 
- Create a route for user registration (/register)
- Implement the following validations using express-validator:
  - Username: not empty, minimum 3 characters
  - Email: not empty, valid email format
  - Password: not empty, minimum 8 characters
  - Phone: optional, but if provided, must be a valid phone number
- Use a custom validator to check if the username or email already exists in the database
- Hash the password using bcrypt before saving it to the database
- Return appropriate error messages for validation failures
- On successful registration, create and send a JWT token in a cookie

### 4. User Login
- Create a route for user login (/login)
- Implement the following validations:
  - Email: not empty, valid email format
  - Password: not empty
- Verify the user's credentials:
  - Check if the user exists in the database
  - Use bcrypt to compare the provided password with the stored hash
- On successful login, create and send a JWT token in a cookie

### 5. Protected Route
- Create a protected route (/profile) that requires authentication
- Implement middleware to verify the JWT token from the request cookie
- The route should return the user's profile information (excluding the password)

### 7. Error Handling and Response Formatting
- Implement proper error handling for all routes
- Format your responses consistently with jsend, including success messages and error messages

## Bonus Challenges
1. Implement password reset functionality
2. Add role-based authorization (e.g., admin and regular user roles)

## Submission Guidelines
- Submit your code as a GitHub repository
- Include a README.md file with instructions on how to set up and run your project