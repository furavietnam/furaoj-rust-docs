# FuraOJ Axum REST API v2 Specification

The Axum REST API exposes versioned JSON endpoints rooted at `/api/v2`.

## Authentication

All protected requests must supply an `Authorization: Bearer <jwt_token>` header.

### `POST /api/v2/auth/login`
- **Request Body:**
  ```json
  { "username": "admin", "password": "admin123" }
  ```
- **Response (200 OK):**
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": { "id": 1, "username": "admin", "email": "admin@furaoj.org", "is_staff": true, "is_superuser": true }
  }
  ```

### `GET /api/v2/auth/me`
- **Response (200 OK):** User profile of authenticated identity.

---

## Problems API

### `GET /api/v2/problems`
- **Query Parameters:** `page` (integer), `keyword` (string).
- **Response (200 OK):** Array of problem objects with code, title, limits, and points.

### `GET /api/v2/problems/:code`
- **Response (200 OK):** Detailed problem statement, constraints, and point values.

---

## Submissions API

### `GET /api/v2/submissions`
- **Query Parameters:** `page`, `problem_code`, `username`, `verdict`.
- **Response (200 OK):** Array of recent submissions.

### `POST /api/v2/submissions`
- **Request Body:**
  ```json
  {
    "problem_id": 1,
    "language": "cpp",
    "source_code": "#include <iostream>..."
  }
  ```
- **Response (200 OK):** Created submission record with initial `QU` status.

### `GET /api/v2/submissions/:id`
- **Response (200 OK):** Detailed test case breakdowns, execution runtimes, and submitted source code.
