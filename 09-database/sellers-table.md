# Sellers Table Specification

Document ID: MLH-DB-003
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `sellers` table used to store marketplace seller and cooperative business information.

The table extends the platform user identity with seller-specific business information, verification status, marketplace configuration, and seller lifecycle data.

---

# Table Name

`sellers`

---

# Description

The `sellers` table stores business-level information for sellers operating on Maroc Local Hub.

A seller may represent:

- An Individual Seller
- A Cooperative
- A Farmer
- An Artisan
- A Manufacturer
- A Local Brand
- A Small Business

The `users` table remains responsible for user identity and authentication.

The `sellers` table is responsible for marketplace business information.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique seller identifier |
| `user_id` | UUID | Yes | Foreign Key, Not Null | Reference to the owner user |
| `business_name` | VARCHAR(255) | Yes | Not Null | Official or registered business name |
| `display_name` | VARCHAR(255) | Yes | Not Null | Public seller name displayed to customers |
| `seller_type` | VARCHAR(50) | Yes | Not Null | Seller category |
| `description` | TEXT | No | Nullable | Seller business description |
| `logo_url` | TEXT | No | Nullable | Seller logo URL |
| `cover_image_url` | TEXT | No | Nullable | Seller storefront cover image |
| `phone_number` | VARCHAR(30) | No | Nullable | Business phone number |
| `email` | VARCHAR(255) | No | Nullable | Business email address |
| `website_url` | TEXT | No | Nullable | External business website |
| `country_code` | VARCHAR(10) | Yes | Not Null | Seller country |
| `region` | VARCHAR(150) | No | Nullable | Administrative region |
| `city` | VARCHAR(150) | No | Nullable | Seller city |
| `address` | TEXT | No | Nullable | Business address |
| `verification_status` | VARCHAR(50) | Yes | Not Null | Seller verification state |
| `status` | VARCHAR(30) | Yes | Not Null | Seller account status |
| `rating_average` | DECIMAL(3,2) | No | Nullable | Average seller rating |
| `rating_count` | INTEGER | Yes | Default 0 | Number of seller ratings |
| `commission_rate` | DECIMAL(5,2) | No | Nullable | Marketplace commission rate |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Record update timestamp |
| `deleted_at` | TIMESTAMP | No | Nullable | Soft delete timestamp |

---

# Seller Types

Supported seller types should include:

- `individual`
- `cooperative`
- `farmer`
- `artisan`
- `manufacturer`
- `local_brand`
- `small_business`

---

# Verification Status

Supported values should include:

- `pending`
- `under_review`
- `verified`
- `rejected`
- `expired`

Seller verification may require additional documents stored in a separate seller verification table or document management system.

---

# Seller Status

Supported values should include:

- `pending`
- `active`
- `suspended`
- `disabled`
- `closed`

---

# Field Notes

## `user_id`

References the owner or primary account associated with the seller.

Recommended relationship:

`users.id → sellers.user_id`

The exact ownership model should be expanded in future versions to support multiple users managing the same seller account.

---

## `business_name`

Stores the legal or registered business name when applicable.

---

## `display_name`

Stores the public-facing seller name displayed throughout the marketplace.

---

## `seller_type`

Defines the category of seller operating on the platform.

---

## `verification_status`

Determines whether the seller has completed the platform verification process.

Only verified sellers should be eligible for marketplace publishing unless explicitly approved by administrators.

---

## `commission_rate`

Stores the commission percentage applied to seller transactions when a custom seller-specific rate is required.

The platform should define a default commission rate separately.

Historical commission rates should be preserved for financial reporting when required.

---

# Indexes

The following indexes are recommended:

- Unique index on `user_id` if one user can own only one seller account.
- Index on `business_name`
- Index on `display_name`
- Index on `seller_type`
- Index on `verification_status`
- Index on `status`
- Index on `country_code`
- Index on `region`
- Index on `city`
- Index on `created_at`
- Index on `deleted_at`

Search indexes may be added later for advanced seller discovery.

---

# Constraints

The table should enforce:

- `user_id` must reference a valid user.
- `business_name` must not be empty.
- `display_name` must not be empty.
- `seller_type` must contain a valid supported value.
- `verification_status` must contain a valid supported value.
- `status` must contain a valid supported value.
- `rating_average` should remain between 0 and 
