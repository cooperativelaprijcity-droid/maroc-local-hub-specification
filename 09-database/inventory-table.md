# Inventory Table Specification

Document ID: MLH-DB-006
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the inventory data model used to manage product stock across Maroc Local Hub.

The inventory system must support accurate stock tracking, seller inventory management, stock reservations, inventory movements, and future multi-warehouse operations.

---

# Table Name

`inventory`

---

# Description

The `inventory` table stores the current stock state of products or product variants.

Inventory management should be separated from the `products` table because one product may have:

- Multiple Variants
- Multiple Warehouses
- Multiple Stock Locations
- Multiple Inventory Movements

The inventory system must provide accurate stock availability while preserving historical inventory records.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique inventory record identifier |
| `product_id` | UUID | Yes | Foreign Key, Not Null | Associated product |
| `variant_id` | UUID | No | Foreign Key, Nullable | Associated product variant |
| `warehouse_id` | UUID | No | Foreign Key, Nullable | Associated warehouse |
| `quantity_on_hand` | INTEGER | Yes | Default 0 | Physical stock currently available |
| `quantity_reserved` | INTEGER | Yes | Default 0 | Stock reserved for pending orders |
| `quantity_available` | INTEGER | Yes | Derived or maintained | Stock available for sale |
| `reorder_level` | INTEGER | No | Default 0 | Stock level that triggers replenishment |
| `status` | VARCHAR(30) | Yes | Not Null | Inventory status |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Record update timestamp |

---

# Inventory Quantity Model

The available quantity should generally follow:

```text
quantity_available =
quantity_on_hand - quantity_reserved
```

The application or database must ensure that available stock does not become negative.

---

# Inventory Status

Supported values should include:

- `in_stock`
- `low_stock`
- `out_of_stock`
- `pre_order`
- `discontinued`

The exact status should be derived from inventory quantities and product configuration where possible.

---

# Field Notes

## `product_id`

References the product associated with the inventory record.

Recommended relationship:

`products.id → inventory.product_id`

---

## `variant_id`

References a specific product variant.

This field should be used when a product has variants such as:

- Size
- Color
- Weight
- Packaging

Example:

```text
Product: Moroccan Honey

Variants:
- 250g
- 500g
- 1kg
```

Each variant may have separate inventory.

---

## `warehouse_id`

References the physical or virtual location where stock is held.

This field may remain nullable during the initial marketplace implementation if inventory is managed directly by sellers without warehouse-level tracking.

Future versions should support multiple warehouses.

---

## `quantity_on_hand`

Represents the physical quantity currently held in stock.

This value should be updated through controlled inventory operations.

---

## `quantity_reserved`

Represents stock temporarily reserved for orders that have not yet been completed.

Reserved inventory should not be available for new purchases.

---

## `quantity_available`

Represents the quantity that can currently be purchased.

The system should prevent overselling by validating available inventory during checkout and order processing.

---

## `reorder_level`

Defines the minimum stock threshold at which the seller should consider replenishment.

Future versions may trigger automatic notifications when stock reaches this level.

---

# Inventory Movements

Changes to inventory should be recorded in a separate table:

`inventory_movements`

Examples include:

- Initial Stock
- Purchase
- Sale
- Return
- Cancellation
- Manual Adjustment
- Damaged Stock
- Lost Stock
- Transfer

Each inventory movement should record:

- Movement ID
- Inventory ID
- Movement Type
- Quantity
- Reference Type
- Reference ID
- Previous Quantity
- New Quantity
- Created By
- Created At

Historical inventory movements should not be deleted or modified without a controlled audit process.

---

# Stock Reservation

The inventory system should support stock reservation during checkout.

A typical workflow is:

1. Customer adds product to cart.
2. Customer begins checkout.
3. System verifies available stock.
4. Stock may be temporarily reserved.
5. Payment is completed.
6. Reserved stock becomes sold stock.

If checkout fails or the reservation expires:

1. Reservation is released.
2. `quantity_reserved` decreases.
3. Stock becomes available again.

Reservation expiration should be configurable.

---

# Order Integration

Inventory must integrate with order processing.

Typical flow:

```text
Order Created
      ↓
Stock Reserved
      ↓
Payment Confirmed
      ↓
Stock Deducted
      ↓
Order Fulfilled
```

For cancelled or refunded orders, inventory adjustments should follow the business rules defined by the order and refund workflows.

---

# Returns

Returned products may be:

- Returned to Available Stock
- Marked as Damaged
- Sent for Inspection
- Written Off

The return condition should be recorded in the inventory movement history.

---

# Multi-Warehouse Support

Future versions should support:

- Multiple Warehouses
- Seller Warehouses
- Regional Warehouses
- Fulfillment Centers
- Stock Transfers

Inventory records should be uniquely associated with the appropriate product, variant, and warehouse combination.

Recommended unique combination:

```text
product_id + variant_id + warehouse_id
```

The exact database constraint should account for nullable values.

---

# Indexes

The following indexes are recommended:

- Index on `product_id`
- Index on `variant_id`
- Index on `warehouse_id`
- Index on `status`
- Index on `quantity_available`
- Index on `updated_at`

A unique composite index should be considered for:

```text
product_id + variant_id + warehouse_id
```

where applicable.

---

# Constraints

The table should enforce:

- `quantity_on_hand` must not be negative.
- `quantity_reserved` must not be negative.
- `quantity_available` must not be negative.
- Reserved quantity should not exceed on-hand quantity.
- Product references must be valid.
- Variant references must be valid when present.
- Warehouse references must be valid when present.

---

# Concurrency and Overselling Prevention

Inventory updates must be safe under concurrent transactions.

The system should use appropriate database transaction controls to prevent two customers from purchasing the same final unit simultaneously.

Possible strategies include:

- Row-Level Locking
- Atomic Updates
- Database Transactions
- Optimistic Concurrency Control

The final implementation should be selected based on the backend architecture.

---

# Security Considerations

Inventory data must be protected from unauthorized modification.

Requirements:

- Sellers can manage inventory for their own products.
- Administrators can manage inventory when authorized.
- Inventory adjustments should be logged.
- Manual stock changes should identify the responsible user.
- Historical inventory movements should be auditable.

---

# Performance Notes

Inventory data may be queried frequently during:

- Product Browsing
- Product Details
- Cart Operations
- Checkout
- Order Processing

The system should optimize inventory reads and writes.

Caching may be used for product availability, but checkout must always validate current inventory against the authoritative database.

---

# Data Lifecycle

Inventory records should follow a lifecycle such as:

1. Inventory created.
2. Initial stock added.
3. Stock updated through inventory movements.
4. Stock reserved for orders.
5. Stock deducted after successful purchase.
6. Stock restored after eligible cancellations or returns.
7. Inventory archived when the product is discontinued.

Historical movement records should remain available for auditing.

---

# Future Enhancements

Future versions may support:

- Automatic Reordering
- Supplier Management
- Purchase Orders
- Multi-Warehouse Fulfillment
- Stock Transfers
- Inventory Forecasting
- AI Demand Forecasting
- Low-Stock Alerts
- Barcode Scanning
- QR Code Inventory
- Batch and Lot Tracking
- Expiration Date Tracking

---

# Related Documents

Related database specifications include:

- `products-table.md`
- `orders-table.md`
- `order-items-table.md`
- `sellers-table.md`
- `database-relationships.md`
- `indexing-strategy.md`

---

# Conclusion

The `inventory` table provides the foundation for reliable stock management within Maroc Local Hub.

The design separates current inventory state from historical inventory movements and product catalog data.

The inventory system must prioritize accuracy, concurrency safety, data integrity, and scalability while supporting future multi-warehouse and AI-powered inventory capabilities.

---

End of Document
