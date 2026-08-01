# Order Items Table Specification

Document ID: MLH-DB-008
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `order_items` table used to store the individual products and product variants included in customer orders.

The table preserves a historical snapshot of the purchased item at the time the order was created.

This ensures that historical orders remain accurate even if the original product is later modified, renamed, repriced, archived, or deleted.

---

# Table Name

`order_items`

---

# Description

The `order_items` table represents the individual line items belonging to an order.

Each order may contain one or more order items.

An order item should preserve important information about the product at the time of purchase, including:

- Product Reference
- Variant Reference
- Seller Reference
- Product Name Snapshot
- SKU Snapshot
- Unit Price
- Quantity
- Discount
- Tax
- Total Amount

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique order item identifier |
| `order_id` | UUID | Yes | Foreign Key, Not Null | Parent order |
| `seller_id` | UUID | Yes | Foreign Key, Not Null | Seller responsible for the item |
| `product_id` | UUID | Yes | Foreign Key, Not Null | Original product reference |
| `variant_id` | UUID | No | Foreign Key, Nullable | Product variant reference |
| `product_name_snapshot` | VARCHAR(255) | Yes | Not Null | Product name at purchase time |
| `sku_snapshot` | VARCHAR(100) | No | Nullable | SKU at purchase time |
| `variant_name_snapshot` | VARCHAR(255) | No | Nullable | Variant name at purchase time |
| `quantity` | INTEGER | Yes | Not Null | Quantity purchased |
| `unit_price` | DECIMAL(12,2) | Yes | Not Null | Unit price at purchase time |
| `discount_amount` | DECIMAL(12,2) | Yes | Default 0 | Discount applied to this item |
| `tax_amount` | DECIMAL(12,2) | Yes | Default 0 | Tax applied to this item |
| `total_amount` | DECIMAL(12,2) | Yes | Not Null | Final line item amount |
| `currency_code` | VARCHAR(10) | Yes | Not Null | Currency used for the item |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |

---

# Relationship with Orders

Each order item belongs to exactly one order.

Recommended relationship:

`orders.id → order_items.order_id`

An order may contain multiple order items.

Example:

```text
Order #MLH-2026-000001
│
├── Moroccan Daghmous Honey 500g × 2
├── Argan Oil 100ml × 1
└── Cactus Seed Oil 30ml × 1
```

---

# Relationship with Products

Each order item references the original product through:

`product_id`

Recommended relationship:

`products.id → order_items.product_id`

The product reference provides traceability.

However, historical order calculations must not depend exclusively on the current product record.

The product may later be:

- Renamed
- Repriced
- Archived
- Suspended
- Deleted

The order item must continue to preserve the original purchase information.

---

# Product Snapshot

The following fields should preserve historical product information:

- `product_name_snapshot`
- `sku_snapshot`
- `variant_name_snapshot`

Additional snapshot fields may be introduced when required.

Examples include:

- Brand Name
- Product Image URL
- Product Attributes
- Product Weight
- Product Origin

The exact snapshot strategy should be determined based on reporting and customer order-history requirements.

---

# Pricing Model

The order item should preserve the exact financial values used at checkout.

The line item calculation should generally follow:

```text
gross_amount =
unit_price × quantity
```

Then:

```text
total_amount =
gross_amount
- discount_amount
+ tax_amount
```

The exact calculation must follow the platform's finalized pricing and tax rules.

All monetary calculations must use precise decimal arithmetic.

Floating-point types should not be used for financial values.

---

# Quantity

The `quantity` field represents the number of units purchased.

Requirements:

- Must be greater than zero.
- Must be an integer for physical products unless fractional quantities are explicitly supported.
- Must be validated against available inventory.

---

# Seller Relationship

Each order item should reference the seller responsible for fulfilling the item.

Recommended relationship:

`sellers.id → order_items.seller_id`

This is particularly important for multi-vendor marketplace orders.

A single customer order may contain products from multiple sellers.

Example:

```text
Customer Order
│
├── Seller A
│    ├── Product 1
│    └── Product 2
│
└── Seller B
     ├── Product 3
     └── Product 4
```

The final architecture may use:

- `orders`
- `seller_orders`
- `order_items`

to separate customer-level orders from seller-level fulfillment.

---

# Currency

The `currency_code` field preserves the currency used at the time of purchase.

Historical order items must not change if the platform later changes:

- Default Currency
- Exchange Rates
- Seller Pricing
- Marketplace Pricing

---

# Discounts

Discounts may originate from:

- Product Discount
- Seller Discount
- Coupon
- Platform Promotion
- Campaign

The order item should preserve the final discount amount applied to that item.

If detailed discount attribution is required, a dedicated table may be introduced:

`order_item_discounts`

---

# Taxes

The `tax_amount` field stores the tax applied to the individual order item.

Future implementations may require additional fields such as:

- Tax Rate
- Tax Type
- Tax Jurisdiction

If detailed tax reporting is required, tax information should be modeled separately.

---

# Inventory Integration

Order items should be linked indirectly to inventory operations.

When an order is confirmed:

1. Inventory is checked.
2. Stock is reserved.
3. Order item is confirmed.
4. Payment is processed.
5. Inventory is deducted according to the finalized order workflow.

Inventory movement records should reference the relevant:

- Order ID
- Order Item ID
- Product ID
- Variant ID

This allows stock changes to be traced back to individual purchases.

---

# Returns and Refunds

An order item may be partially or fully returned.

The system should support:

- Full Item Return
- Partial Quantity Return
- Full Refund
- Partial Refund

Future implementations may introduce:

`order_item_returns`

and:

`refund_items`

to provide detailed return and refund tracking.

---

# Status

The initial design does not require a dedicated item status field if the order lifecycle is sufficient.

However, a dedicated `status` field may be introduced when the platform supports:

- Partial Fulfillment
- Partial Cancellation
- Item-Level Returns
- Item-Level Refunds

Possible values include:

- `pending`
- `confirmed`
- `fulfilled`
- `cancelled`
- `returned`
- `refunded`

---

# Indexes

The following indexes are recommended:

- Index on `order_id`
- Index on `seller_id`
- Index on `product_id`
- Index on `variant_id`
- Index on `created_at`

Composite indexes may be introduced based on real query patterns.

---

# Constraints

The table should enforce:

- `order_id` must reference a valid order.
- `seller_id` must reference a valid seller.
- `product_id` must reference a valid product where historical references are retained.
- `quantity` must be greater than zero.
- `unit_price` must not be negative.
- `discount_amount` must not be negative.
- `tax_amount` must not be negative.
- `total_amount` must not be negative.
- `currency_code` must be valid.

---

# Data Integrity

Order item financial data should remain immutable after order completion except through controlled financial operations.

Corrections should be handled through:

- Refunds
- Returns
- Adjustment Transactions

Historical purchase prices should not be changed by modifying the current product price.

---

# Security Considerations

Order item information contains commercial and customer transaction data.

Access must follow the order access policy.

Requirements:

- Customers can access their own order items.
- Sellers can access items belonging to their products.
- Administrators can access order items according to permissions.
- Financial information must be protected.
- Historical records must be auditable.

---

# Data Lifecycle

An order item follows a lifecycle such as:

1. Added to order.
2. Order confirmed.
3. Inventory reserved.
4. Payment confirmed.
5. Item fulfilled.
6. Item delivered.
7. Order completed.

Alternative paths include:

- Cancelled
- Returned
- Refunded
- Partially Refunded

Historical order items should be retained according to business and legal requirements.

---

# Performance Notes

The `order_items` table may grow significantly over time.

The database should support efficient queries for:

- Customer order history.
- Seller order management.
- Product sales analytics.
- Inventory reconciliation.
- Financial reporting.

Indexes should be reviewed periodically based on actual query patterns.

---

# Future Enhancements

Future versions may support:

- Item-Level Fulfillment
- Item-Level Shipping
- Item-Level Returns
- Item-Level Refunds
- Product Bundles
- Subscription Items
- Digital Product Items
- AI-Based Sales Analytics

---

# Related Documents

Related database specifications include:

- `orders-table.md`
- `products-table.md`
- `sellers-table.md`
- `inventory-table.md`
- `payments-table.md`
- `shipping-table.md`
- `database-relationships.md`

---

# Conclusion

The `order_items` table provides the detailed product-level record for every customer purchase.

Its most important responsibility is preserving historical purchase information independently from the current product catalog.

This ensures that order history, financial reporting, seller accounting, inventory reconciliation, and customer support remain accurate throughout the lifecycle of Maroc Local Hub.

---

End of Document
