# AirBnB Clone Backend Requirements Specification

This document outlines the technical and functional requirements for three key backend features of the AirBnB Clone project: **User Authentication**, **Property Management**, and **Booking System**. Each feature includes functional and technical requirements, API endpoints, input/output specifications, validation rules, and performance criteria.

## 1. User Authentication

### Functional Requirements
- **F1.1**: Users (Guests and Hosts) can register with an email, password, and role.
- **F1.2**: Users (Guests, Hosts, Admins) can log in with email and password to receive a session token.
- **F1.3**: Users can update their profile (e.g., name, phone number).
- **F1.4**: Role-based access control ensures Guests access booking features, Hosts manage properties, and Admins manage users and disputes.

### Technical Requirements
- **T1.1**: Implement a RESTful API using a framework (e.g., Node.js/Express, Flask).
- **T1.2**: Use JWT (JSON Web Tokens) for session management, with tokens valid for 24 hours.
- **T1.3**: Store passwords using bcrypt hashing with a salt factor of 12.
- **T1.4**: Use PostgreSQL for the `users` table (fields: `user_id`, `first_name`, `last_name`, `email`, `password_hash`, `phone_number`, `role`, `created_at`).
- **T1.5**: Implement HTTPS for secure communication.
- **T1.6**: Log authentication attempts for security auditing.

### API Endpoints

#### 1.1 Register User
- **Endpoint**: `POST /api/auth/register`
- **Description**: Creates a new user account.
- **Request Body** (JSON):
  ```json
  {
    "first_name": "string",
    "last_name": "string",
    "email": "string",
    "password": "string",
    "phone_number": "string",
    "role": "string" // "guest" or "host"
  }
  ```
- **Response** (JSON):
  - **201 Created**:
    ```json
    {
      "user_id": "UUID",
      "email": "string",
      "role": "string",
      "created_at": "ISO8601 timestamp"
    }
    ```
  - **400 Bad Request**: `{ "error": "Invalid input" }`
  - **409 Conflict**: `{ "error": "Email already exists" }`
- **Validation Rules**:
  - `first_name`, `last_name`: Non-empty, max 50 characters.
  - `email`: Valid email format, max 100 characters, unique.
  - `password`: Min 8 characters, includes uppercase, lowercase, number, special character.
  - `phone_number`: Optional, valid format (e.g., +1-555-555-5555), max 20 characters.
  - `role`: Must be "guest" or "host".
- **Performance Criteria**:
  - Response time: < 200ms for 95% of requests.
  - Throughput: Handle 1000 registrations/hour.
  - Scalability: Support 10,000 concurrent users with horizontal scaling.

#### 1.2 Login User
- **Endpoint**: `POST /api/auth/login`
- **Description**: Authenticates a user and returns a JWT.
- **Request Body** (JSON):
  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```
- **Response** (JSON):
  - **200 OK**:
    ```json
    {
      "user_id": "UUID",
      "email": "string",
      "role": "string",
      "token": "JWT string"
    }
    ```
  - **401 Unauthorized**: `{ "error": "Invalid credentials" }`
- **Validation Rules**:
  - `email`: Valid email format, exists in database.
  - `password`: Matches stored hash.
- **Performance Criteria**:
  - Response time: < 150ms for 95% of requests.
  - Throughput: Handle 5000 logins/hour.
  - Security: Rate-limit to 5 attempts per IP per minute.

#### 1.3 Update Profile
- **Endpoint**: `PUT /api/auth/profile`
- **Description**: Updates user profile information.
- **Headers**: `Authorization: Bearer <JWT>`
- **Request Body** (JSON):
  ```json
  {
    "first_name": "string",
    "last_name": "string",
    "phone_number": "string"
  }
  ```
- **Response** (JSON):
  - **200 OK**:
    ```json
    {
      "user_id": "UUID",
      "first_name": "string",
      "last_name": "string",
      "phone_number": "string"
    }
    ```
  - **400 Bad Request**: `{ "error": "Invalid input" }`
  - **401 Unauthorized**: `{ "error": "Invalid token" }`
- **Validation Rules**:
  - `first_name`, `last_name`: Non-empty, max 50 characters.
  - `phone_number`: Optional, valid format, max 20 characters.
  - JWT: Valid, not expired, matches user_id.
- **Performance Criteria**:
  - Response time: < 200ms for 95% of requests.
  - Throughput: Handle 1000 updates/hour.

## 2. Property Management

### Functional Requirements
- **F2.1**: Hosts can create, update, and delete property listings.
- **F2.2**: Guests can search properties by location, price, or dates.
- **F2.3**: System stores property details, including location data.

### Technical Requirements
- **T2.1**: RESTful API for property CRUD operations.
- **T2.2**: Use PostgreSQL for `properties` (fields: `property_id`, `host_id`, `location_id`, `name`, `description`, `pricepernight`, `created_at`, `updated_at`) and `locations` (fields: `location_id`, `street_address`, `city`, `state`, `country`, `postal_code`).
- **T2.3**: Implement indexing on `properties(location_id, pricepernight)` and `locations(city, country)` for search performance.
- **T2.4**: Authenticate requests using JWT, restricting property management to Hosts.
- **T2.5**: Cache search results using Redis for frequently queried locations.

### API Endpoints

#### 2.1 Create Property
- **Endpoint**: `POST /api/properties`
- **Description**: Creates a new property listing.
- **Headers**: `Authorization: Bearer <JWT>`
- **Request Body** (JSON):
  ```json
  {
    "name": "string",
    "description": "string",
    "pricepernight": "number",
    "location": {
      "street_address": "string",
      "city": "string",
      "state": "string",
      "country": "string",
      "postal_code": "string"
    }
  }
  ```
- **Response** (JSON):
  - **201 Created**:
    ```json
    {
      "property_id": "UUID",
      "host_id": "UUID",
      "location_id": "UUID",
      "name": "string",
      "pricepernight": "number",
      "created_at": "ISO8601 timestamp"
    }
    ```
  - **400 Bad Request**: `{ "error": "Invalid input" }`
  - **401 Unauthorized**: `{ "error": "Invalid token" }`
- **Validation Rules**:
  - `name`: Non-empty, max 255 characters.
  - `description`: Non-empty, max 5000 characters.
  - `pricepernight`: Positive number, max 10000.00, 2 decimal places.
  - `location.city`, `location.country`: Non-empty, max 100 characters.
  - `location.street_address`, `location.state`, `location.postal_code`: Optional, max 255/100/20 characters.
  - JWT: Valid, user role must be "host".
- **Performance Criteria**:
  - Response time: < 250ms for 95% of requests.
  - Throughput: Handle 500 creations/hour.

#### 2.2 Search Properties
- **Endpoint**: `GET /api/properties/search?city=<string>&min_price=<number>&max_price=<number>&start_date=<date>&end_date=<date>`
- **Description**: Searches properties by criteria.
- **Request Query Parameters**:
  - `city`: Optional, string.
  - `min_price`, `max_price`: Optional, numbers.
  - `start_date`, `end_date`: Optional, ISO8601 dates (e.g., 2025-06-01).
- **Response** (JSON):
  - **200 OK**:
    ```json
    [
      {
        "property_id": "UUID",
        "name": "string",
        "pricepernight": "number",
        "city": "string",
        "country": "string"
      }
    ]
    ```
  - **400 Bad Request**: `{ "error": "Invalid query parameters" }`
- **Validation Rules**:
  - `city`: Max 100 characters.
  - `min_price`, `max_price`: Positive numbers, `min_price <= max_price`.
  - `start_date`, `end_date`: Valid dates, `start_date < end_date`, within 1 year.
- **Performance Criteria**:
  - Response time: < 100ms for 95% of requests (cached results).
  - Throughput: Handle 10,000 searches/hour.
  - Cache hit ratio: > 80% for frequent queries.

## 3. Booking System

### Functional Requirements
- **F3.1**: Guests can book a property for specific dates.
- **F3.2**: System validates availability and calculates total price.
- **F3.3**: Guests can view their booking history.

### Technical Requirements
- **T3.1**: RESTful API for booking creation and retrieval.
- **T3.2**: Use PostgreSQL for `bookings` (fields: `booking_id`, `property_id`, `user_id`, `start_date`, `end_date`, `total_price`, `status`, `created_at`).
- **T3.3**: Implement indexing on `bookings(property_id, user_id, start_date)` for performance.
- **T3.4**: Use transactions to ensure booking and payment consistency.
- **T3.5**: Authenticate requests using JWT, restricting booking to Guests.

### API Endpoints

#### 3.1 Create Booking
- **Endpoint**: `POST /api/bookings`
- **Description**: Creates a new booking.
- **Headers**: `Authorization: Bearer <JWT>`
- **Request Body** (JSON):
  ```json
  {
    "property_id": "UUID",
    "start_date": "ISO8601 date",
    "end_date": "ISO8601 date"
  }
  ```
- **Response** (JSON):
  - **201 Created**:
    ```json
    {
      "booking_id": "UUID",
      "property_id": "UUID",
      "user_id": "UUID",
      "start_date": "ISO8601 date",
      "end_date": "ISO8601 date",
      "total_price": "number",
      "status": "pending"
    }
    ```
  - **400 Bad Request**: `{ "error": "Invalid input" }`
  - **401 Unauthorized**: `{ "error": "Invalid token" }`
  - **409 Conflict**: `{ "error": "Property not available" }`
- **Validation Rules**:
  - `property_id`: Valid UUID, exists in `properties`.
  - `start_date`, `end_date`: Valid dates, `start_date < end_date`, within 1 year.
  - System: Property must be available (no overlapping bookings).
  - JWT: Valid, user role must be "guest".
- **Performance Criteria**:
  - Response time: < 300ms for 95% of requests.
  - Throughput: Handle 2000 bookings/hour.
  - Concurrency: Prevent double bookings using database locks.

#### 3.2 View Booking History
- **Endpoint**: `GET /api/bookings`
- **Description**: Retrieves a user’s booking history.
- **Headers**: `Authorization: Bearer <JWT>`
- **Response** (JSON):
  - **200 OK**:
    ```json
    [
      {
        "booking_id": "UUID",
        "property_id": "UUID",
        "start_date": "ISO8601 date",
        "end_date": "ISO8601 date",
        "total_price": "number",
        "status": "string"
      }
    ]
    ```
  - **401 Unauthorized**: `{ "error": "Invalid token" }`
- **Validation Rules**:
  - JWT: Valid, matches requesting user.
- **Performance Criteria**:
  - Response time: < 150ms for 95% of requests.
  - Throughput: Handle 5000 requests/hour.