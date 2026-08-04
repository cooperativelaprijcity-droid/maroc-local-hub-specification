# API Architecture

Document ID: MLH-ARCH-005

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the API architecture for Maroc Local Hub.

It specifies how frontend applications, backend services, third-party integrations, and future mobile applications communicate through standardized, secure, and scalable APIs.

---

# Objectives

The API architecture should:

- Provide consistent communication
- Support secure authentication
- Enable frontend/backend separation
- Support third-party integrations
- Ensure scalability
- Simplify maintenance
- Support future API versions

---

# API Style

The platform primarily uses:

- REST APIs

Future versions may additionally support:

- GraphQL
- WebSockets
- Event APIs

---

# Design Principles

The API should follow:

- Stateless communication
- Resource-oriented endpoints
- Predictable URLs
- Standard HTTP methods
- JSON request/response format
- Versioning support
- Secure authentication
- Consistent error handling

---

# HTTP Methods

Supported methods include:

- GET
- POST
- PUT
- PATCH
- DELETE

Each endpoint should use the appropriate HTTP method.

---

# API Versioning

Recommended version format:

```
/api/v1/
```

Example:

```
GET /api/v1/products
```

Future versions:

```
/api/v2/
```

Older versions should remain available according to the platform's deprecation policy.

---

# Resource Groups

The API should be organized into logical resources.

Examples include:

- Authentication
- Users
- Sellers
- Products
- Categories
- Inventory
- Cart
- Orders
- Payments
- Shipping
- Coupons
- Reviews
- Notifications
- Administration

---

# Authentication

Protected endpoints require authentication.

Authentication should use secure access tokens.

Public endpoints include:

- Login
- Registration
- Product browsing
- Categories
- Public seller profiles

---

# Authorization

Access should be controlled through Role-Based Access Control (RBAC).

Examples:

- Customer
- Seller
- Administrator
- Moderator
- Support Agent

Authorization must be verified on every protected request.

---

# Request Format

Requests should use JSON.

Example:

```json
{
  "name": "Organic Honey",
  "price": 120.00,
  "currency": "MAD"
}
```

---

# Response Format

Successful responses should return structured JSON.

Example:

```json
{
  "success": true,
  "data": {
    "id": "UUID",
    "name": "Organic Honey"
  }
}
```

---

# Error Responses

Errors should follow a consistent structure.

Example:

```json
{
  "success": false,
  "error": {
    "code": "PRODUCT_NOT_FOUND",
    "message": "Requested product does not exist."
  }
}
```

Internal implementation details must never be exposed.

---

# Pagination

Collection endpoints should support pagination.

Recommended parameters:

- page
- limit

Optional parameters:

- sort
- order
- filter
- search

---

# Filtering

Example:

```
GET /api/v1/products?category=honey
```

---

# Sorting

Example:

```
GET /api/v1/products?sort=price&order=asc
```

---

# Searching

Example:
