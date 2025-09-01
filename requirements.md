# Backend Requirement Specifications – Airbnb Clone

This document details the **functional** and **technical requirements** for three key backend features of the Airbnb Clone system.

---

## 1. User Authentication

### Objective

Enable users (Guests, Hosts, Admins) to securely register, log in, and manage sessions.

### API Endpoints

| Method | Endpoint             | Description                                           |
| ------ | -------------------- | ----------------------------------------------------- |
| `POST` | `/api/auth/register` | Registers a new user                                  |
| `POST` | `/api/auth/login`    | Authenticates a user and issues JWT token             |
| `GET`  | `/api/auth/profile`  | Fetches current logged-in user profile (JWT required) |
| `POST` | `/api/auth/logout`   | Invalidates current session/token                     |

### Input / Output Specifications

**Register Request:**

```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Register Response (201 Created):**

```json
{
  "message": "User registered successfully",
  "user_id": 101
}
```

**Login Request:**

```json
{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Login Response (200 OK):**

```json
{
  "token": "JWT_TOKEN_HERE",
  "expires_in": 3600
}
```

### Validation Rules

* Email must be unique and match standard email format.
* Password must be at least 8 characters, include 1 uppercase letter, 1 number, and 1 special character.
* All fields required during registration.

### Performance Criteria

* Authentication responses must complete in < 500 ms.
* JWT token expiration set to 1 hour; refresh token supported.

---

## 2. Property Management

### Objective

Allow hosts to **create, update, and manage property listings** while enabling users to browse and filter them efficiently.

### API Endpoints

| Method   | Endpoint              | Description                                          |
| -------- | --------------------- | ---------------------------------------------------- |
| `POST`   | `/api/properties`     | Create a new property (Host only)                    |
| `GET`    | `/api/properties`     | Fetch all properties (supports filters & pagination) |
| `GET`    | `/api/properties/:id` | Fetch single property details                        |
| `PUT`    | `/api/properties/:id` | Update property details (Host only)                  |
| `DELETE` | `/api/properties/:id` | Remove property listing (Host only)                  |

### Input / Output Specifications

**Create Property Request:**

```json
{
  "title": "Modern Apartment in Berlin",
  "description": "Cozy 2-bedroom apartment near city center.",
  "price": 75,
  "location": "Berlin, Germany",
  "capacity": 4,
  "amenities": ["WiFi", "Heating", "Kitchen"]
}
```

**Create Property Response (201 Created):**

```json
{
  "message": "Property created successfully",
  "property_id": 501
}
```

**Fetch Properties Response (200 OK):**

```json
[
  {
    "id": 501,
    "title": "Modern Apartment in Berlin",
    "price": 75,
    "location": "Berlin, Germany",
    "capacity": 4,
    "amenities": ["WiFi", "Heating", "Kitchen"],
    "available": true
  }
]
```

### Validation Rules

* **Title**: Required, max 100 characters.
* **Price**: Positive number, cannot exceed system-defined max.
* **Location**: Required, must be a valid string.
* **Capacity**: Positive integer, minimum 1.
* **Amenities**: Must be an array of strings.

### Performance Criteria

* Property search (with filters) must return in < 700 ms for up to 10,000 records.
* Pagination supported (default 10 results per page).
* Caching recommended for frequently accessed property data.

---

## 3. Booking System

### Objective

Enable users to book properties, manage booking statuses, and prevent double-booking with proper concurrency handling.

### API Endpoints

| Method | Endpoint                   | Description                                       |
| ------ | -------------------------- | ------------------------------------------------- |
| `POST` | `/api/bookings`            | Create a booking (requires property ID and dates) |
| `GET`  | `/api/bookings`            | View all bookings for the logged-in user          |
| `GET`  | `/api/bookings/:id`        | Fetch details of a specific booking               |
| `PUT`  | `/api/bookings/:id/cancel` | Cancel a booking (if cancellation policy allows)  |

### Input / Output Specifications

**Create Booking Request:**

```json
{
  "property_id": 501,
  "check_in": "2025-09-05",
  "check_out": "2025-09-10",
  "guests": 2
}
```

**Create Booking Response (201 Created):**

```json
{
  "message": "Booking created successfully",
  "booking_id": 9001,
  "status": "Pending"
}
```

**Cancel Booking Response (200 OK):**

```json
{
  "message": "Booking cancelled successfully",
  "status": "Cancelled"
}
```

### Validation Rules

* **Date Validation**: check\_in must be earlier than check\_out.
* **Availability Check**: Property must not have overlapping bookings for the date range.
* **Guest Limit**: Must not exceed property capacity.

### Performance Criteria

* Booking creation and availability check must complete in < 1 second.
* System must handle concurrent booking requests without race conditions (use DB transactions/locking).
* Scalability to support 10,000+ bookings per day.

---

## General Notes

* All API endpoints must return JSON responses.
* Authentication is required for creating/updating properties and managing bookings.
* Error Handling with proper HTTP status codes:

  * `400 Bad Request` (invalid input)
  * `401 Unauthorized` (missing/invalid token)
  * `404 Not Found` (resource not found)
  * `500 Internal Server Error`
