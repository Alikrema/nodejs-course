---
layout: page
title: Session 3
parent: Assignments
---
## Building a Simple Movie Management API

### Objective

Create a backend server that manages movie data using Express.js. This project will guide you through understanding and applying Service-Oriented Architecture (SOA) principles to build a robust and maintainable API.

### Project Structure

The project will be organized to follow best practices in separating concerns, ensuring that each part of the application has a specific role and responsibility. The key components will include:

- **Controllers:** Handle the HTTP requests and responses.
- **Services:** Encapsulate the business logic.
- **Middlewares:** Process requests before they reach the controllers.
- **Models:** Represent the data and handle data operations.
- **Routes:** Define the API endpoints.

### Step-by-Step Guide

### 1. Define the Objective

You will create an API that allows users to manage movie data. The API will support the following operations:

- Retrieve a list of all movies
- Retrieve details of a specific movie by ID
- Add a new movie
- Update an existing movie
- Delete a movie

### 2. Apply Service-Oriented Architecture (SOA)

**Service-Oriented Architecture (SOA)** is a design principle where your application is organized into services. Each service is responsible for a specific piece of functionality. In this project, you'll separate the application into different layers to achieve modularity and maintainability.

### 3. Structure Your Application

Your application will have the following key components:

- **Controllers:** These will handle incoming HTTP requests and send responses back to the client. They will delegate the actual processing to services.
- **Services:** These will contain the business logic. Controllers will call these services to perform actions like fetching data, updating data, etc.
- **Middlewares:** These will be functions that process requests before they reach the controllers. Common examples include logging, authentication, and error handling.
- **Models:** These will represent the movie data and handle data operations like reading from and writing to data storage.
- **Routes:** These will define the URL endpoints for your API and map them to the corresponding controller functions.
- DAL/Repos

### 4. Implement the Features

For each feature, think about the following steps:

1. **Routes:** Define the API endpoints.
2. **Controllers:** Implement the functions that will be called when these endpoints are hit.
3. **Services:** Write the business logic that the controllers will call.
4. **Models:** Manage the movie data.

### 5. Consider Middleware

Middleware functions can enhance your API by adding common processing steps for requests. Examples include:

- **Logging:** Keep track of API calls for debugging and monitoring.
- **Error Handling:** Standardize how errors are managed and returned to the client.

### Deliverables

By the end of the task, you should have a fully functional Movie Management API with:

- A well-structured codebase following SOA principles.
- Routes to handle CRUD operations for movies.
- Controllers that manage request and response flows.
- Services encapsulating the business logic.
- Middleware for logging and error handling.
- Models to represent and manage movie data.
- DAL/Repos contains database connection and operations (mock data)