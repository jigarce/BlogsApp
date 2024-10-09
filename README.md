# 📝 BlogsApp - .NET Core Microservices Application

**BlogsApp** is a simple .NET Core-based microservices app that showcases various features such as creating a service that allows for:
- Creating new posts with comments
- Fetching posts by ID, along with the post comments.

The app also uses JWT tokens for authentication, rate-limits each API call, and uses Redis for caching to improve performance. The entire app is containerized in Docker, along with its dependencies, including SQL Server and Redis.

Below is a high-level architecture diagram that shows the various components of this app and how they are structured:

![Architecture Diagram](https://github.com/jigarce/BlogsApp/blob/master/BlogAPP.png)

## 🔍 Components Overview

Here is a brief summary of each component and what it does:

### 🌐 Client Apps
- **Browsers/Mobile Apps**: Interface through which users interact with the application. They send requests to and receive responses from the Application Layer.

### 📂 Application Layer - WebApp
- **Posts Controller**: Acts as an API endpoint. It processes incoming HTTP requests, invokes the appropriate actions in the Service Layer, and returns HTTP responses.

### 🛠️ Services - CQRS Pattern
- **IPostCommandService**: Handles all write operations (create, update, delete) for posts and comments, following the Command aspect of CQRS.
- **IPostQueryService**: Manages read operations (retrieve posts and comments), embodying the Query aspect of CQRS.

### 💾 Data Layer
- **MS SQL Server**: Primary persistent data store for posts and comments.
- **Redis Server**: Provides fast, in-memory caching, reducing the load on SQL Server and improving read performance.

### 🐳 Infrastructure Layer
- **Docker**: Containerizes and isolates the Application and Data Layers, ensuring consistent and easy deployment across environments.

Each component operates within its scope, collaboratively supporting the application's functionality while adhering to the separation of concerns dictated by the CQRS pattern.

---

## 🚀 Running the App

You can run and test the app using **Postman**. Follow these steps:

1. 🛠️ **[Download](https://github.com/jigarce/BlogsApp/blob/master/Postman_collection.json)** the Postman collection JSON.
2. 📥 Import the collection into your Postman app.
3. Use the following APIs in the specified order:

    - **Step 1**: 🔑 Get an Auth Token:
      ```http
      POST http://myblog.azurecontainer.io:8080/api/auth/token
      ```
    
    - **Step 2**: 📝 Create a new blog post with comments (use the JSON structure below):
      ```http
      POST http://myblog.azurecontainer.io:8080/api/posts/
      ```

      ```json
      {
        "title": "Azure DB Entry 1",
        "content": "This should work",
        "comments": [
          {
            "author": "Supreet",
            "content": "Does this work?"
          }
        ]
      }
      ```

    - **Step 3**: 🔍 Fetch a post by ID:
      ```http
      GET http://myblog.azurecontainer.io:8080/api/posts/{id}
      ```

## 📝 Additional Notes

- Posts are fetched from the database on the first request and cached afterward. 
- The default rate limit for APIs is **5 requests per minute**.
