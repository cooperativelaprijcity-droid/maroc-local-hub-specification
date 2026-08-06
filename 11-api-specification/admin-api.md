# Admin API

Document ID: MLH-API-015

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Admin API for Maroc Local Hub.

It specifies all endpoints available to platform administrators for managing users, sellers, products, categories, orders, payments, reports, platform settings, and operational monitoring.

---

# Objectives

The Admin API should:

- Manage platform operations
- Moderate marketplace content
- Monitor business performance
- Configure platform settings
- Manage users and sellers
- Access reports and analytics
- Maintain platform security

---

# Base URL

```
/api/v1/admin
```

---

# Modules

The Admin API includes:

- Dashboard
- User Management
- Seller Management
- Product Management
- Category Management
- Order Management
- Payment Management
- Coupon Management
- Review Moderation
- Reports & Analytics
- Platform Settings
- Audit Logs

---

# Dashboard

### Endpoint

```
GET /api/v1/admin/dashboard
```

Returns:

- Total Users
- Total Sellers
- Total Products
- Total Orders
- Total Revenue
- Pending Verifications
- Active Promotions
- Platform Health

---

# User Management

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /users | Get users |
| GET | /users/{id} | Get user details |
| PATCH | /users/{id}/status | Update user status |
| DELETE | /users/{id} | Delete user |

---

# Seller Management

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /sellers | Get sellers |
| GET | /sellers/{id} | Seller details |
| PATCH | /sellers/{id}/verify | Verify seller |
| PATCH | /sellers/{id}/status | Update seller status |

---

# Product Management

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /products | Get products |
| PATCH | /products/{id}/status | Update product status |
| DELETE | /products/{id} | Delete product |
| POST | /products/{id}/feature | Feature product |

---

# Category Management

| Method | Endpoint | Description |
|---------|----------|-------------|
| POST | /categories | Create category |
| PUT | /categories/{id} | Update category |
| DELETE | /categories/{id} | Delete category |

---

# Order Management

Administrative actions include:

- View Orders
- Update Status
- Cancel Orders
- Approve Returns
- Process Refunds

---

# Payment Management

Supported operations:

- View Transactions
- Verify Payments
- Approve Refunds
- Export Financial Reports

---

# Coupon Management

Supported operations:

- Create Campaigns
- Activate Coupons
- Disable Coupons
- View Statistics

---

# Review Moderation

Administrators may:

- Approve Reviews
- Hide Reviews
- Delete Reviews
- Resolve Reports

---

# Reports & Analytics

Reports may include:

- Sales Reports
- Revenue Reports
- User Growth
- Seller Performance
- Product Performance
- Traffic Statistics
- Conversion Rates

---

# Platform Settings

Settings may include:

- General Configuration
- Marketplace Rules
- Payment Configuration
- Shipping Configuration
- Notification Templates
- Localization
- Security Policies

---

# Audit Logs

### Endpoint

```
GET /api/v1/admin/audit-logs
```

Logs include:

- User Actions
- Administrator Actions
- Authentication Events
- Configuration Changes
- Security Events

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

Administrator privileges are mandatory.

---

# Authorization Roles

Supported roles:

- Super Admin
- Platform Admin
- Support Agent
- Finance Manager
- Content Moderator

---

# Validation Rules

Examples include:

- Administrator permissions required
- Resource must exist
- Action must be authorized
- Input validation required

---

# Error Responses

Possible errors include:

- UNAUTHORIZED
- FORBIDDEN
- RESOURCE_NOT_FOUND
- VALIDATION_ERROR
- OPERATION_NOT_ALLOWED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Access Control (RBAC)
- Multi-Factor Authentication (Future)
- Audit Logging
- Session Management
- IP Restrictions (Optional)

---

# Future Enhancements

Future improvements may include:

- AI-assisted moderation
- Automated fraud detection
- Business intelligence dashboards
- Predictive analytics
- Bulk administrative actions

---

# Related Documents

- authentication-api.md
- users-api.md
- sellers-api.md
- orders-api.md
- ../10-architecture/security-architecture.md

---

# Conclusion

The Admin API provides a secure and comprehensive administrative interface for managing all operational aspects of the Maroc Local Hub platform while ensuring governance, security, and scalability.

---

End of Document
