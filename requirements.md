# Backend Requirement Specifications – Airbnb Clone

This document details the **functional** and **technical requirements** for three key backend features of the Airbnb Clone system.

---

## 1. User Authentication

### Objective
Enable users (Guests, Hosts, Admins) to securely register, log in, and manage sessions.

### API Endpoints
| Method | Endpoint           | Description |
|-------|------------------|-------------|
| `POST` | `/api/auth/register` | Registers a new user |
| `POST` | `/api/auth/login` | Authenticates a user and issues JWT token |
| `GET`  | `/api/auth/profile` | Fetches current logged-in user profile (JWT required) |
| `POST` | `/api/auth/logout` | Invalidates current session/token |

### Input / Output Specifications
**Register Request (JSON):**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
