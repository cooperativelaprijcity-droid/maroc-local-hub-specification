# Sellers API

Document ID: MLH-API-004

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Sellers API for Maroc Local Hub.

It specifies all endpoints related to seller registration, store management, verification, dashboard access, and seller operations.

---

# Objectives

The Sellers API should:

- Register new sellers
- Manage seller profiles
- Manage store information
- Support verification workflows
- Display seller statistics
- Enable seller account management

---

# Base URL

```
/api/v1/sellers
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| POST | /register | Register as a seller |
| GET | /me | Get seller profile |
| PUT | /me | Update seller profile |
| GET | /dashboard | Get seller dashboard |
| GET | /statistics | Get seller statistics |
| GET | /verification | Verification status |
| POST | /verification | Submit verification documents |
| PUT | /store | Update store information |
| GET | /store | Get store information |
| DELETE | /me | Delete seller account |

---

# Seller Registration

### Endpoint

```
POST /api/v1/sellers/register
```

### Request Body

```json
{
  "storeName": "Atlas Honey",
  "businessType": "Cooperative",
  "email": "seller@example.com",
  "phone": "+212600000000",
  "country": "Morocco"
}
```

### Success Response

```json
{
  "success": true,
  "message": "Seller registration submitted successfully."
}
```

---

# Seller Profile

### Endpoint

```
GET /api/v1/sellers/me
```

Returns:

- Seller ID
- Store Name
- Business Type
- Verification Status
- Rating
- Join Date

---

# Update Seller Profile

### Endpoint

```
PUT /api/v1/sellers/me
```

Allows updating:

- Store Name
- Contact Information
- Business Description
- Logo
- Banner
- Social Links

---

# Store Information

Store information may include:

- Store Name
- Description
- Logo
- Cover Image
- Address
- Business Hours
- Contact Information
- Shipping Policies
- Return Policies

---

# Seller Dashboard

### Endpoint

```
GET /api/v1/sellers/dashboard
```

Dashboard data may include:

- Total Orders
- Revenue
- Products
- Pending Orders
- Reviews
- Average Rating
- Monthly Sales

---

# Seller Statistics

### Endpoint

```
GET /api/v1/sellers/statistics
```

Statistics may include:

- Total Sales
- Revenue
- Conversion Rate
- Best Selling Products
- Customer Growth
- Refund Rate

---

# Verification

Verification status:

- Pending
- Approved
- Rejected

Required documents may include:

- National ID
- Business Registration
- Cooperative Certificate
- Tax Documents (if applicable)

---

# Delete Seller Account

### Endpoint

```
DELETE /api/v1/sellers/me
```

Seller account deletion should follow platform policies and administrative review where applicable.

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Validation Rules

Examples include:

- Unique Store Name
- Valid Contact Information
- Required Verification Documents
- Accepted Image Formats

---

# Error Responses

Possible errors include:

- UNAUTHORIZED
- VALIDATION_ERROR
- STORE_ALREADY_EXISTS
- VERIFICATION_PENDING
- RESOURCE_NOT_FOUND

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- Input Validation
- Secure File Upload
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- Multi-store management
- Seller analytics dashboard
- Financial reports
- Seller subscription plans
- API access for enterprise sellers

---

# Related Documents

- users-api.md
- ../09-database/sellers-table.md
- ../08-features/seller-dashboard.md

---

# Conclusion

The Sellers API enables secure and efficient management of sellers and cooperative stores, providing the foundation for marketplace operations within Maroc Local Hub.

---

End of Document
