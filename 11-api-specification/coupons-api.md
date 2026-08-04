# Coupons API

Document ID: MLH-API-012

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Coupons API for Maroc Local Hub.

It specifies all endpoints related to coupon creation, validation, application, expiration, campaign management, and promotional discounts.

---

# Objectives

The Coupons API should:

- Create promotional coupons
- Validate coupon codes
- Apply discounts securely
- Support marketing campaigns
- Track coupon usage
- Prevent coupon abuse

---

# Base URL

```
/api/v1/coupons
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get coupons |
| GET | /{id} | Get coupon details |
| POST | / | Create coupon |
| PUT | /{id} | Update coupon |
| DELETE | /{id} | Delete coupon |
| POST | /validate | Validate coupon |
| POST | /apply | Apply coupon to cart |
| GET | /statistics | Coupon statistics |

---

# Get Coupons

### Endpoint

```
GET /api/v1/coupons
```

Supports:

- Pagination
- Filtering
- Status
- Campaign

---

# Get Coupon Details

### Endpoint

```
GET /api/v1/coupons/{id}
```

Returns:

- Coupon Code
- Discount Type
- Discount Value
- Status
- Expiration Date
- Usage Limit
- Remaining Uses

---

# Create Coupon

### Endpoint

```
POST /api/v1/coupons
```

### Request Body

```json
{
  "code": "WELCOME10",
  "discountType": "Percentage",
  "discountValue": 10,
  "expirationDate": "2027-01-01",
  "usageLimit": 500
}
```

---

# Update Coupon

### Endpoint

```
PUT /api/v1/coupons/{id}
```

Allows updating:

- Discount
- Expiration
- Usage Limits
- Status

---

# Delete Coupon

### Endpoint

```
DELETE /api/v1/coupons/{id}
```

Removes or archives the coupon according to business policy.

---

# Validate Coupon

### Endpoint

```
POST /api/v1/coupons/validate
```

### Request Body

```json
{
  "code": "WELCOME10",
  "cartId": 25
}
```

Validation includes:

- Coupon exists
- Active status
- Expiration
- Usage limits
- Minimum order value
- Customer eligibility

---

# Apply Coupon

### Endpoint

```
POST /api/v1/coupons/apply
```

Returns:

- Discount Amount
- Updated Cart Total
- Applied Coupon

---

# Coupon Types

Supported types:

- Percentage Discount
- Fixed Amount Discount
- Free Shipping
- Buy One Get One (Future)

---

# Coupon Status

Supported values:

- Active
- Scheduled
- Expired
- Disabled

---

# Statistics

### Endpoint

```
GET /api/v1/coupons/statistics
```

Returns:

- Total Coupons
- Active Coupons
- Redemption Rate
- Total Discount Value
- Campaign Performance

---

# Validation Rules

Examples include:

- Coupon code must be unique
- Expiration date must be in the future
- Discount must be valid
- Usage limit cannot be negative

---

# Authentication

Administrative endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

Public validation and application endpoints require an authenticated customer.

---

# Error Responses

Possible errors include:

- COUPON_NOT_FOUND
- COUPON_EXPIRED
- COUPON_ALREADY_USED
- INVALID_COUPON
- MINIMUM_ORDER_NOT_MET
- USAGE_LIMIT_REACHED
- VALIDATION_ERROR

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Rate Limiting
- Coupon Abuse Detection
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- Personalized coupons
- AI-powered promotions
- Referral rewards
- Loyalty coupons
- Automatic discount campaigns

---

# Related Documents

- cart-api.md
- orders-api.md
- payments-api.md

---

# Conclusion

The Coupons API enables secure and flexible promotional campaigns while improving customer engagement and supporting marketing initiatives across the Maroc Local Hub marketplace.

---

End of Document
