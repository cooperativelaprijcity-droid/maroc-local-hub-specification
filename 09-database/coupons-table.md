# Coupons Table Specification

Document ID: MLH-DB-011
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `coupons` table used to manage discount codes and promotional offers within Maroc Local Hub.

The coupon system should support platform-wide promotions, seller-specific coupons, percentage discounts, fixed discounts, usage limits, expiration dates, and minimum order requirements.

---

# Table Name

`coupons`

---

# Description

The `coupons` table stores coupon and promotional code definitions.

Coupons may be created by:

- Platform Administrators
- Authorized Sellers

Coupons may apply to:

- Entire Marketplace
- Specific Sellers
- Specific Products
- Specific Categories

The exact scope should be managed through dedicated relationship tables where necessary.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique coupon identifier |
| `code` | VARCHAR(100) | Yes | Unique, Not Null | Coupon code entered by customer |
| `name` | VARCHAR(255) | Yes | Not Null | Internal coupon name |
| `description` | TEXT | No | Nullable | Coupon description |
| `discount_type` | VARCHAR(30) | Yes | Not Null | Percentage or fixed discount |
| `discount_value` | DECIMAL(12,2) | Yes | Not Null | Discount amount or percentage |
| `currency_code` | VARCHAR(10) | No | Nullable | Currency for fixed discounts |
| `minimum_order_amount` | DECIMAL(12,2) | No | Nullable | Minimum order value |
| `maximum_discount_amount` | DECIMAL(12,2) | No | Nullable | Maximum discount allowed |
| `usage_limit` | INTEGER | No | Nullable | Maximum total uses |
| `usage_limit_per_user` | INTEGER | No | Nullable | Maximum uses per customer |
| `used_count` | INTEGER | Yes | Default 0 | Number of successful uses |
| `starts_at` | TIMESTAMP | Yes | Not Null | Coupon activation time |
| `expires_at` | TIMESTAMP | No | Nullable | Coupon expiration time |
| `seller_id` | UUID | No | Foreign Key | Seller who owns the coupon |
| `status` | VARCHAR(30) | Yes | Not Null | Coupon status |
| `created_by` | UUID | Yes | Foreign Key | User who created the coupon |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Last update timestamp |
| `deleted_at` | TIMESTAMP | No | Nullable | Soft delete timestamp |

---

# Discount Types

Supported values should include:

- `percentage`
- `fixed_amount`

Example percentage discount:

```text
discount_type = percentage
discount_value = 10
```

This represents a 10% discount.

Example fixed discount:

```text
discount_type = fixed_amount
discount_value = 50
currency_code = MAD
```

This represents a 50 MAD discount.

---

# Coupon Status

Supported values should include:

- `draft`
- `scheduled`
- `active`
- `expired`
- `disabled`
- `archived`

---

# Coupon Code

The coupon `code` should be:

- Unique
- Case-insensitive where appropriate
- Easy to enter
- Easy to communicate

Examples:

```text
WELCOME10
GUELMIM20
MOROCCO15
SUMMER2026
```

The platform should define whether codes are stored normalized in uppercase.

Recommended approach:

Store coupon codes in a normalized format and compare them case-insensitively.

---

# Coupon Scope

Coupons may apply to different scopes.

Possible scopes include:

- `platform`
- `seller`
- `product`
- `category`

A dedicated field may be introduced:

`scope_type`

The relationship between coupons and products, sellers, or categories should be handled using dedicated junction tables.

Recommended tables include:

- `coupon_products`
- `coupon_categories`
- `coupon_sellers`

---

# Seller Coupons

Sellers may create coupons for their own products if authorized.

Seller-created coupons must be restricted to products owned or managed by that seller.

The system must validate seller ownership before allowing coupon creation or modification.

---

# Platform Coupons

Administrators may create platform-wide coupons.

Examples:

- New Customer Discount
- Seasonal Promotion
- Marketplace Campaign
- Holiday Discount

Platform coupons may apply to multiple sellers.

---

# Minimum Order Amount

The `minimum_order_amount` field defines the minimum order value required to use the coupon.

Example:

```text
minimum_order_amount = 300 MAD
```

The coupon can only be applied when the eligible order value meets the minimum requirement.

---

# Maximum Discount Amount

For percentage-based coupons, a maximum discount amount may be configured.

Example:

```text
Discount: 20%
Maximum Discount: 100 MAD
```

This prevents large discounts on high-value orders.

---

# Usage Limits

The system should support:

- Total Usage Limit
- Per User Usage Limit

Example:

```text
usage_limit = 1000
usage_limit_per_user = 1
```

This allows the coupon to be used up to 1,000 times, with each customer using it only once.

---

# Coupon Usage Tracking
