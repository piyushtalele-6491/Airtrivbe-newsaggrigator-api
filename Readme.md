# News Aggregator API

Welcome to the News Aggregator API project! This repository contains the implementation of a RESTful API built using Node.js, Express.js, and various NPM packages. The API allows users to register, log in, set news preferences, and fetch news articles from multiple sources based on their preferences. Additionally, optional extensions such as caching, marking articles as "read" or "favorite," and searching for news articles have been implemented.

- The API enables users to register, log in, and define their news preferences seamlessly.
- News articles are retrieved from various sources through external APIs, specifically NewsAPI.
- The received articles undergo asynchronous processing and filtering based on individual user preferences, with results being cached to optimize and minimize API calls.
- The cache management utilizes the Least Recently Used (LRU) algorithm, prioritizing the storage of frequently accessed articles.
- Periodic updates to the cache are facilitated by a cron job, triggering an API call every 20 minutes to ensure data freshness.
- User authentication and endpoint protection are enforced through the use of JSON Web Tokens (JWT).
- Rigorous input validation is implemented for user registration, login, and preference updates to enhance data integrity.
- Thorough error handling mechanisms address invalid requests and authentication issues effectively.
- The implementation incorporates a rate limiter using IP addresses, allowing a maximum of 10 requests within a 2-second window to maintain system stability.

# Installation

To run the application, ensure Node.js is installed on your machine.

- Clone the repository: git clone https://github.com/piyushtalele-6491/Airtrivbe-newsaggrigator-api/edit/main/Readme.md
- Install dependencies: npm install
- Start the server: node src/server.js
  The application should be running at http://localhost:3001.

# Running the Tests

    npm test

# Usage

Access the application at http://localhost:3001 in your web browser. The in-memory DB is created inside the dev-data folder, and a users.json file is generated.

The application enables the following operations:

- Register: Send a POST request to /register with a JSON body containing the user's email and password to create a new user. The password is securely hashed using bcrypt before storage.

- Login: Send a POST request to /login with a JSON body containing the user's email and password. A valid response includes a JWT token.

- Update Preferences: Send a PUT request to /preferences with the JWT token in the Authorization header and a JSON body containing updated preferences.

- Fetch News: Send a GET request to /news with the JWT token in the Authorization header to retrieve news articles based on the user's preferences. The API fetches and filters articles using the NewsAPI, implementing the LRU algorithm for caching.

- Mark as Read/Favorite: Send a POST request to /news/:id/read or /news/:id/favorite with the article ID in the URL parameter and the JWT token in the Authorization header.

- Get Read/Favorite Articles: Send a GET request to /news/read or /news/favorites with the JWT token in the Authorization header.

- Search Articles: Send a GET request to /news/search/:keyword with the keyword in the URL parameter and the JWT token in the Authorization header.

# Folder Structure

- controllers: Separate controller files for different application logic aspects.

  - authController.js: Manages user authentication and route protection using JWT tokens and middleware functions.
  - errorController.js: Handles global error handling.
  - newsController.js: Manages news-related actions, including fetching and caching articles.
  - userController.js: Handles user-related operations.

- helpers: Contains helper files and modules.

  - apiHelper.js: Wrapper for making Axios calls to third-party APIs.
  - cron.js: Updates the news cache every 20 minutes.
  - customError.js: Custom error class for application-specific errors.
  - fileHelper.js: Functions for file operations like reading and writing JSON files.
  - getNewsHelper.js: Functions for making GET API calls to third-party news sources.
  - lruCache.js: Implements the LRU algorithm for caching news articles.
  - validationHelper.js: Functions for basic validation in API responses.

- routes: Router files for different API parts.

  - newsRouter.js: Routes for fetching news articles and updating the news cache.
  - userRouter.js: Routes for getting the user list, updating user preferences, and getting preferences for a specific user.

- validators: Validation files using Express-validator for user sign-in, login, and preference validations.

- .env files: Contains environment variables for the news API key and JWT secret.

- tests: Automated tests for controllers, helpers, and routes.

# API Endpoints

- POST /register: Register a new user.
- POST /login: Log in a user.
- GET /preference: Retrieve news preferences for the logged-in user.
- PUT /preference: Update news preferences for the logged-in user.
- GET /news: Fetch news articles based on the logged-in user's preferences.
- POST /news/:id/read: Mark a news article as read.
- POST /news/:id/favorite: Mark a news article as a favorite.
- GET /news/read: Retrieve all read news articles.
- GET /news/favorites: Retrieve all favorite news articles.
- GET /news/search/:keyword: Search for news articles based on keywords.

# API Collection

# Error Handling

Proper error handling for invalid requests is implemented, providing clear error messages with corresponding status codes.

# Input Validation

Input validation is enforced for user creation and preference updates. Allowed news preferences include "business," "entertainment," "general," "health," "science," "sports," and "technology."

# Cache

- The cache, stored in cache.json, employs the LRU algorithm to prioritize frequently accessed articles.
- Periodic updates occur via a cron job every 20 minutes, fetching fresh articles and reducing external API calls.
- When a user requests news articles, the API first checks the cache. If not present or expired, it fetches, processes, filters, and stores the articles in the cache.
- Modifiable cache parameters are found in lruCache.js.

# Rate Limiting

Rate limiting, implemented using a fixed window counter algorithm, restricts requests from a single IP address to 10 within a 2-second window. This helps prevent service overload and potential malicious attacks.

Adjustable parameters are available in rateLimiter.js.

# Testing the API

Use Postman or cURL to send HTTP requests to API endpoints. Test the cache by sending multiple requests to /news and observe cached article returns for subsequent requests within the cache expiration period. Modify cache-related configurations in lruCache.js.
