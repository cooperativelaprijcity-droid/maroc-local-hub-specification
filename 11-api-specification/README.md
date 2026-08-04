# API Specification

Document ID: MLH-API-001

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This directory contains the complete REST API specification for Maroc Local Hub.

It defines all API endpoints, request and response structures, authentication mechanisms, validation rules, error handling, versioning strategy, and integration guidelines.

This documentation serves as the primary reference for backend, frontend, mobile, and third-party developers.

---

# Objectives

The API specification aims to:

- Standardize communication between clients and servers
- Ensure consistent API behavior
- Simplify frontend and mobile development
- Enable secure integrations
- Improve maintainability
- Support future scalability

---

# API Design Principles

The APIs should follow these principles:

- RESTful Architecture
- Resource-Oriented Design
- Stateless Requests
- Predictable Endpoints
- Secure by Default
- Versioned APIs
- Consistent Naming
- Consistent Error Responses

---

# Authentication

Protected endpoints require authentication.

Supported mechanisms include:

- JWT Access Tokens
- Refresh Tokens
- OAuth 2.0 (Future)

---

# API Versioning

The first public version uses:

```
/api/v1/
```

Future versions:

```
/api/v2/
```

---

# Response Format

All endpoints should return JSON.

Example:

```json
{
  "success": true,
  "message": "Request completed successfully.",
  "data": {}
}
```

---

# Error Format

Example:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request."
  }
}
```

---

# HTTP Status Codes

Common status codes:

- 200 OK
- 201 Created
- 204 No Content
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 409 Conflict
- 422 Unprocessable Entity
- 429 Too Many Requests
- 500 Internal Server Error

---

# API Modules

This directory contains specifications for:

- Authentication API
- Users API
- Sellers API
- Products API
- Categories API
- Inventory API
- Cart API
- Orders API
- Payments API
- Shipping API
- Coupons API
- Reviews API
- Notifications API
- Admin API

---

# Naming Convention

Resources should use plural nouns.

Examples:

```
/users
/products
/orders
/categories
```

---

# Security

APIs should support:

- HTTPS
- JWT Authentication
- Input Validation
- Rate Limiting
- Role-Based Authorization
- Audit Logging

---

# Documentation Standard

Each API document should include:

- Purpose
- Endpoint List
- Request Parameters
- Request Body
- Response Body
- Validation Rules
- Authentication Requirements
- Error Responses
- Example Requests
- Example Responses

---

# Related Documents

- ../10-architecture/api-architecture.md
- ../10-architecture/security-architecture.md
- ../09-database/database-overview.md

---

# Conclusion

The API Specification provides a standardized foundation for implementing secure, scalable, and maintainable REST APIs across the Maroc Local Hub platform.

---

End of Document
