# Reviews API

Document ID: MLH-API-013

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Reviews API for Maroc Local Hub.

It specifies all endpoints related to customer reviews, product ratings, seller ratings, moderation, reporting, and review management.

---

# Objectives

The Reviews API should:

- Allow customers to review products
- Allow customers to rate sellers
- Improve marketplace trust
- Prevent review abuse
- Support moderation
- Display accurate rating statistics

---

# Base URL

```
/api/v1/reviews
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /products/{productId} | Get product reviews |
| GET | /sellers/{sellerId} | Get seller reviews |
| POST | / | Create review |
| PUT | /{id} | Update review |
| DELETE | /{id} | Delete review |
| POST | /{id}/report | Report review |
| GET | /statistics | Review statistics |

---

# Get Product Reviews

### Endpoint

```
GET /api/v1/reviews/products/{productId}
```

Supports:

- Pagination
- Rating filter
- Sorting
- Verified purchase filter

---

# Get Seller Reviews

### Endpoint

```
GET /api/v1/reviews/sellers/{sellerId}
```

Returns:

- Seller Rating
- Review Count
- Customer Comments
- Rating Distribution

---

# Create Review

### Endpoint

```
POST /api/v1/reviews
```

### Request Body

```json
{
  "productId": 15,
  "rating": 5,
  "title": "Excellent Product",
  "comment": "Very high quality and fast delivery."
}
```

### Success Response

```json
{
  "success": true,
  "message": "Review submitted successfully."
}
```

---

# Update Review

### Endpoint

```
PUT /api/v1/reviews/{id}
```

Allows updating:

- Rating
- Title
- Comment

---

# Delete Review

### Endpoint

```
DELETE /api/v1/reviews/{id}
```

Customers may delete their own reviews according to platform policy.

---

# Report Review

### Endpoint

```
POST /api/v1/reviews/{id}/report
```

Reasons may include:

- Spam
- Offensive Content
- Fake Review
- Inappropriate Language
- Conflict of Interest

---

# Review Statistics

### Endpoint

```
GET /api/v1/reviews/statistics
```

Returns:

- Average Rating
- Total Reviews
- Rating Distribution
- Verified Reviews
- Reported Reviews

---

# Rating Scale

Supported ratings:

- 1 Star
- 2 Stars
- 3 Stars
- 4 Stars
- 5 Stars

---

# Validation Rules

Examples include:

- Rating is required
- Rating must be between 1 and 5
- Comment length limits
- One review per completed order (configurable)

---

# Authentication

Protected endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

Only authenticated users may submit or edit reviews.

---

# Error Responses

Possible errors include:

- REVIEW_NOT_FOUND
- ALREADY_REVIEWED
- INVALID_RATING
- PRODUCT_NOT_FOUND
- UNAUTHORIZED
- VALIDATION_ERROR

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Input Validation
- Review Moderation
- Spam Detection
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- Photo reviews
- Video reviews
- AI-assisted moderation
- Verified purchase badges
- Seller responses
- Helpful vote system

---

# Related Documents

- products-api.md
- sellers-api.md
- orders-api.md

---

# Conclusion

The Reviews API provides a transparent and reliable review system that helps customers make informed purchasing decisions while improving marketplace quality and trust.

---

End of Document
