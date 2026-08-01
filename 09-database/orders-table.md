# Orders Table Specification

Document ID: MLH-DB-007
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `orders` table used to store customer purchase orders within Maroc Local Hub.

The order system must support marketplace transactions involving customers, sellers, products, payments, shipments, discounts, cancellations, refunds, and order lifecycle management.

---

# Table Name

`orders`

---

# Description

The `orders` table stores the main record for each customer order.

An order represents a commercial transaction created when a customer completes the checkout process.

The order record should contain summary information and references to related entities.

Detailed purchased product information must be stored in:

`order_items`

Payment information must be stored in:

`payments`

Shipping information must be stored in:

`shipping`

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique order identifier |
| `order_number` | VARCHAR(50) | Yes | Unique, Not Null | Human-readable order reference |
| `user_id` | UUID | Yes | Foreign Key, Not Null | Customer who created the order |
| `seller_id` | UUID | Yes | Foreign Key, Not Null | Primary seller associated with the order |
| `status` | VARCHAR(40) | Yes | Not Null | Current order status |
| `payment_status` | VARCHAR(40) | Yes | Not Null | Payment state |
| `fulfillment_status` | VARCHAR(40) | Yes | Not Null | Fulfillment state |
| `currency_code` | VARCHAR(10) | Yes | Not Null | Currency used for the order |
| `subtotal` | DECIMAL(12,2) | Yes | Not Null | Total before discounts, shipping, and taxes |
| `discount_amount` | DECIMAL(12,2) | Yes | Default 0 | Total discount amount |
| `shipping_amount` | DECIMAL(12,2) | Yes | Default 0 | Shipping cost |
| `tax_amount` | DECIMAL(12,2) | Yes | Default 0 | Tax amount |
| `total_amount` | DECIMAL(12,2) | Yes | Not Null | Final order total |
| `coupon_id` | UUID | No | Foreign Key | Applied coupon |
| `shipping_address_id` | UUID | No | Foreign Key | Delivery address |
| `billing_address_id` | UUID | No | Foreign Key | Billing address |
| `customer_note` | TEXT | No | Nullable | Customer note |
| `created_at` | TIMESTAMP | Yes | Not Null | Order creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Last update timestamp |
| `cancelled_at` | TIMESTAMP | No | Nullable | Cancellation timestamp |
| `completed_at` | TIMESTAMP | No | Nullable | Completion timestamp |

---

# Order Number

The `order_number` is a human-readable reference shown to customers, sellers, and administrators.

Example:

```text
MLH-2026-000001
```

The order number must be unique.

It should not be used as the primary database identifier.

---

# Order Status

Supported values should include:

- `pending`
- `confirmed`
- `processing`
- `ready_for_shipping`
- `shipped`
- `delivered`
- `completed`
- `cancelled`
- `refunded`
- `partially_refunded`

The final list should align with the platform's order management workflow.

---

# Payment Status

Supported values should include:

- `pending`
- `authorized`
- `paid`
- `partially_paid`
- `failed`
- `refunded`
- `partially_refunded`

Payment status must be managed independently from order status.

---

# Fulfillment Status

Supported values should include:

- `unfulfilled`
- `partially_fulfilled`
- `fulfilled`
- `cancelled`

Fulfillment status represents the physical preparation and delivery state of the order.

---

# Field Notes

## `user_id`

References the customer who placed the order.

Recommended relationship:

`users.id → orders.user_id`

Historical orders should remain available even if the customer later closes or deletes their account.

---

## `seller_id`

References the primary seller associated with the order.

For a marketplace supporting multiple sellers in one checkout, this field may not be sufficient.

In that case, the architecture should introduce:

`order_sellers`

or split the checkout into seller-specific sub-orders.

This decision must be finalized before production implementation.

---

## `currency_code`

Stores the currency used for the order.

The currency must remain unchanged for historical orders.

---

## `subtotal`

Represents the total value of purchased items before:

- Discounts
- Shipping
- Taxes

---

## `discount_amount`

Represents the total discount applied to the order.

This may include:

- Coupon Discounts
- Promotional Discounts
- Seller Discounts
- Platform Discounts

The exact discount allocation should be stored in appropriate transaction or discount records when detailed reporting is required.

---

## `total_amount`

Represents the final amount charged to the customer.

A typical calculation is:

```text
total_amount =
subtotal
- discount_amount
+ shipping_amount
+ tax_amount
```

The final calculation must be performed using precise decimal arithmetic.

---

# Address Data

Orders should preserve the delivery and billing address used at the time of purchase.

The platform should avoid relying exclusively on a user's current profile address because users may change their address after an order is created.

Depending on the final architecture, the system may:

- Store immutable order address snapshots.
- Reference dedicated order address records.

Recommended approach:

`order_addresses`

This preserves historical accuracy.

---

# Order Items

Each order should contain one or more order items.

Order item data should include:

- Product ID
- Variant ID
- Seller ID
- Product Name Snapshot
- SKU Snapshot
- Quantity
- Unit Price
- Discount
- Tax
- Total Price

The detailed schema should be defined in:

`order-items-table.md`

Product names and prices should be stored as snapshots in order items to preserve historical order information.

---

# Order Lifecycle

A typical order lifecycle is:

```text
Cart
  ↓
Checkout
  ↓
Order Created
  ↓
Payment Pending
  ↓
Payment Confirmed
  ↓
Order Processing
  ↓
Ready for Shipping
  ↓
Shipped
  ↓
Delivered
  ↓
Completed
```

Possible alternative paths include:

```text
Payment Failed
      ↓
Order Cancelled
```

or:

```text
Order Created
      ↓
Customer Cancellation
      ↓
Cancelled
```

or:

```text
Delivered
      ↓
Return Request
      ↓
Refund
```

---

# Order Creation

An order should be created only after the checkout process has validated:

- Customer identity
- Product availability
- Inventory availability
- Pricing
- Coupon validity
- Shipping method
- Payment method

The order creation process should occur within a controlled database transaction.

---

# Inventory Integration

When an order is created:

1. Inventory availability is verified.
2. Required stock may be reserved.
3. Order is created.
4. Payment is processed.
5. Inventory is deducted or reservation is confirmed according to the payment workflow.

The system must prevent overselling.

---

# Payment Integration

Orders must be linked to payment records.

An order may have:

- One payment
- Multiple payment attempts
- Partial payments
- Refund transactions

Payment transaction details should be stored separately.

---

# Shipping Integration

An order may have one or more shipment records.

This is especially important for:

- Multi-seller orders
- Split shipments
- Partial fulfillment

Shipping data should be stored in:

`shipping`

---

# Coupons

An order may use a coupon.

The system must preserve the coupon information used at the time of purchase.

If a coupon is later modified or deleted, historical orders must retain their original discount values.

---

# Cancellation

Orders may be cancelled according to defined business rules.

Cancellation may be initiated by:

- Customer
- Seller
- Administrator
- System

The cancellation process should record:

- Cancellation reason
- Cancelled by
- Cancellation timestamp

A dedicated order status history table is recommended.

---

# Order Status History

A separate table is recommended:

`order_status_history`

It should record:

- History ID
- Order ID
- Previous Status
- New Status
- Changed By
- Reason
- Created At

This provides a complete audit trail for order lifecycle changes.

---

# Indexes

The following indexes are recommended:

- Unique index on `order_number`
- Index on `user_id`
- Index on `seller_id`
- Index on `status`
- Index on `payment_status`
- Index on `fulfillment_status`
- Index on `created_at`
- Index on `coupon_id`

Additional composite indexes may be added based on actual query patterns.

---

# Constraints

The table should enforce:

- `order_number` must be unique.
- `user_id` must reference a valid user.
- `seller_id` must reference a valid seller when applicable.
- Monetary values must not be negative unless explicitly allowed.
- `total_amount` must be consistent with order calculations.
- `currency_code` must be valid.
- Order status values must be controlled.
- Payment status values must be controlled.
- Fulfillment status values must be controlled.

---

# Security Considerations

Orders contain sensitive customer and commercial information.

Requirements:

- Customers can access only their own orders.
- Sellers can access only orders related to their products or seller account.
- Administrators can access orders according to their permissions.
- Order data must be protected during transmission and storage.
- Administrative actions should be auditable.

---

# Data Lifecycle

Order records should follow a lifecycle such as:

1. Order created.
2. Payment processing.
3. Payment confirmed.
4. Order processing.
5. Fulfillment.
6. Shipping.
7. Delivery.
8. Completion.

Alternative outcomes may include:

- Cancellation
- Refund
- Partial Refund
- Return

Historical order data should be retained according to business and legal requirements.

---

# Performance Notes

The `orders` table will be frequently queried by:

- Customer dashboards.
- Seller dashboards.
- Administrator dashboards.
- Payment systems.
- Shipping systems.
- Analytics systems.

The database should use appropriate indexes and pagination for large order histories.

---

# Multi-Seller Marketplace Consideration

Maroc Local Hub is designed as a marketplace.

A single customer checkout may potentially contain products from multiple sellers.

The final database architecture should therefore consider a parent-child order structure:

```text
Customer Order
      │
      ├── Seller Order A
      │      ├── Product A
      │      └── Product B
      │
      └── Seller Order B
             ├── Product C
             └── Product D
```

A possible implementation is:

- `orders` = Customer-level parent order.
- `seller_orders` = Seller-specific sub-orders.
- `order_items` = Purchased products.

This architecture is recommended for a true multi-vendor marketplace.

The final decision should be made before database implementation.

---

# Future Enhancements

Future versions may support:

- Multi-Seller Checkout
- Split Orders
- Partial Fulfillment
- Subscription Orders
- Pre-Orders
- Buy Now Pay Later
- Automated Refunds
- Return Management
- AI-Based Order Support
- Order Fraud Detection

---

# Related Documents

Related database specifications include:

- `users-table.md`
- `sellers-table.md`
- `products-table.md`
- `inventory-table.md`
- `order-items-table.md`
- `payments-table.md`
- `shipping-table.md`
- `coupons-table.md`
- `database-relationships.md`

---

# Conclusion

The `orders` table is
