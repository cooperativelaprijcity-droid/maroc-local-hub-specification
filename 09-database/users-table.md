# Users Table Specification

Document ID: MLH-DB-002
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `users` table used to store platform account data for customers, sellers, cooperatives, administrators, and support staff.

The table is the core identity record for authentication, account management, messaging, notifications, wishlist ownership, reviews, orders, and platform access control.

---

# Table Name

`users`

---

# Description

The `users` table stores the primary account record for every authenticated user on the platform.

It contains identity data, authentication data, account status, and basic profile references.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique identifier for the user |
| `email` | VARCHAR(255) | Yes | Unique, Not Null | User email address |
| `phone_number` | VARCHAR(30) | No | Unique | User phone number |
| `password_hash` | TEXT | Yes | Not Null | Securely hashed password |
| `role_code` | VARCHAR(50) | Yes | Not Null | User role such as customer, seller, cooperative, administrator, support_staff |
| `status` | VARCHAR(30) | Yes | Not Null | Account status such as active, pending_verification, suspended, disabled, deleted |
| `email_verified_at` | TIMESTAMP | No | Nullable | Email verification timestamp |
| `phone_verified_at` | TIMESTAMP | No | Nullable | Phone verification timestamp |
| `last_login_at` | TIMESTAMP | No | Nullable | Last successful login timestamp |
| `preferred_language` | VARCHAR(10) | No | Nullable | Preferred language code such as ar, fr, en, es |
| `preferred_currency` | VARCHAR(10) | No | Nullable | Preferred currency code |
| `timezone` | VARCHAR(50) | No | Nullable | User timezone |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Record update timestamp |
| `deleted_at` | TIMESTAMP | No | Nullable | Soft delete timestamp |

---

# Field Notes

## `email`
This field must be unique across the platform.

It is used for login, verification, notifications, and account recovery.

## `phone_number`
This field is optional but recommended for sellers and customers.

It may be used for OTP login, delivery communication, and account recovery.

## `password_hash`
The platform must never store plain text passwords.

Only secure password hashes are allowed.

## `role_code`
This field defines the account category.

Supported values should include:

- `customer`
- `seller`
- `cooperative`
- `administrator`
- `support_staff`

## `status`
This field defines the current state of the account.

Supported values should include:

- `active`
- `pending_verification`
- `suspended`
- `disabled`
- `deleted`

## `preferred_language`
This field supports the multilingual strategy of the platform.

Supported values should include:

- `ar`
- `fr`
- `en`
- `es`

---

# Indexes

The following indexes are recommended:

- Unique index on `email`
- Unique index on `phone_number` where not null
- Index on `role_code`
- Index on `status`
- Index on `created_at`
- Index on `deleted_at`

---

# Constraints

The table should enforce:

- `email` must be unique.
- `password_hash` must not be null.
- `role_code` must not be null.
- `status` must not be null.
- `created_at` and `updated_at` must always be present.
- Soft deletion should preserve historical data.

---

# Relationships

The `users` table is related to several platform tables.

Possible relationships include:

- One user may own one or more seller profiles.
- One user may create one or more orders.
- One user may own one or more wishlist records.
- One user may write one or more reviews.
- One user may participate in one or more conversations.
- One user may receive many notifications.
- One user may have many sessions or authentication records.

Detailed relationships should be documented in `database-relationships.md`.

---

# Security Considerations

The `users` table contains sensitive account data and must be protected carefully.

Requirements:

- Passwords must be hashed using secure modern algorithms.
- Access to user data must be role-restricted.
- Personal information must be protected by access control.
- Deleted accounts should be soft deleted unless legal deletion is required.
- Audit logs should be maintained for important account events.

---

# Data Lifecycle

User records should support the following lifecycle:

1. Account created.
2. Account verified.
3. Account active.
4. Account suspended or disabled if needed.
5. Account soft deleted when appropriate.

---

# Performance Notes

The `users` table will be queried frequently for:

- Authentication
- Profile loading
- Notifications
- Orders
- Messaging
- Wishlist access

The table should therefore be indexed appropriately and kept lean.

---

# Conclusion

The `users` table is the foundation of identity and access management within Maroc Local Hub.

It must be secure, scalable, and aligned with the platform's multilingual, international, and marketplace requirements.
