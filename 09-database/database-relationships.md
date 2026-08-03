# Database Relationships Specification

Document ID: MLH-DB-013
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the relationships between the main database entities of Maroc Local Hub.

The objective is to provide a clear relational model that connects users, sellers, products, inventory, orders, payments, shipping, coupons, and reviews.

The database architecture must support:

- Marketplace Operations
- Multi-Vendor Commerce
- Customer Accounts
- Seller Accounts
- Product Management
- Inventory Management
- Order Management
- Payment Processing
- Shipping
- Promotions
- Reviews
- Analytics

---

# Core Entities

The initial database architecture includes the following core entities:

```text
users
sellers
products
categories
inventory
orders
order_items
payments
shipping
coupons
reviews
```

Additional supporting tables may be introduced as the platform evolves.

---

# High-Level Relationship Model

```text
                         ┌──────────────┐
                         │    users     │
                         └──────┬───────┘
                                │
                 ┌──────────────┼──────────────┐
                 │              │              │
                 ▼              ▼              ▼
          ┌───────────┐   ┌───────────┐   ┌───────────┐
          │  sellers  │   │  orders   │   │  reviews  │
          └─────┬─────┘   └─────┬─────┘   └─────┬─────┘
                │               │               │
                │               │               │
                ▼               ▼               │
          ┌───────────┐   ┌─────────────┐       │
          │ products  │   │ order_items │◄──────┘
          └─────┬─────┘   └──────┬──────┘
                │                │
                ▼                ▼
          ┌───────────┐    ┌────────────┐
          │ inventory │    │  payments  │
          └───────────┘    └────────────┘
                                  │
                                  ▼
                           ┌────────────┐
                           │  shipping  │
                           └────────────┘

          ┌───────────┐
          │  coupons  │
          └─────┬─────┘
                │
                ▼
             orders
```

---

# 1. Users and Sellers

Relationship:

```text
users 1 ─────── 0..N sellers
```

A user may:

- Have no seller account.
- Have one seller account.
- Potentially manage multiple seller entities depending on the final business model.

Recommended relationship:

```text
users.id
    ↓
sellers.user_id
```

The seller record should reference the user account responsible for managing it.

---

# 2. Users and Orders

Relationship:

```text
users 1 ─────── N orders
```

A customer may create multiple orders.

Recommended relationship:

```text
users.id
    ↓
orders.user_id
```

Each order belongs to one customer.

Historical orders should remain traceable according to the platform's data retention policy.

---

# 3. Users and Payments

Relationship:

```text
users 1 ─────── N payments
```

A customer may create multiple payment transactions.

Recommended relationship:

```text
users.id
    ↓
payments.user_id
```

This supports:

- Successful Payments
- Failed Attempts
- Multiple Payment Attempts
- Refund Tracking

---

# 4. Users and Reviews

Relationship:

```text
users 1 ─────── N reviews
```

A customer may create multiple reviews.

Recommended relationship:

```text
users.id
    ↓
reviews.user_id
```

Review permissions must ensure that customers can modify only their own reviews.

---

# 5. Sellers and Products

Relationship:

```text
sellers 1 ─────── N products
```

A seller may manage multiple products.

Recommended relationship:

```text
sellers.id
    ↓
products.seller_id
```

Each marketplace product should have a clear ownership relationship with a seller.

---

# 6. Sellers and Order Items

Relationship:

```text
sellers 1 ─────── N order_items
```

A seller may have many products purchased through different orders.

Recommended relationship:

```text
sellers.id
    ↓
order_items.seller_id
```

This relationship is important for:

- Seller Dashboards
- Seller Fulfillment
- Seller Accounting
- Seller Payouts

---

# 7. Sellers and Shipping

Relationship:

```text
sellers 1 ─────── N shipping
```

A seller may fulfill multiple shipments.

Recommended relationship:

```text
sellers.id
    ↓
shipping.seller_id
```

This supports multi-seller orders and split shipments.

---

# 8. Sellers and Reviews

Relationship:

```text
sellers 1 ─────── N reviews
```

A seller may receive multiple reviews.

Recommended relationship:

```text
sellers.id
    ↓
reviews.seller_id
```

Seller reviews should be distinguishable from product reviews.

---

# 9. Sellers and Coupons

Relationship:

```text
sellers 1 ─────── N coupons
```

A seller may create multiple seller-specific coupons.

Recommended relationship:

```text
sellers.id
    ↓
coupons.seller_id
```

Seller-created coupons must be restricted to authorized sellers.

---

# 10. Products and Categories

Relationship:

```text
categories N ─────── N products
```

A product may belong to one or more categories.

A category may contain many products.

Recommended implementation:

```text
products
    │
    ▼
product_categories
    ▲
    │
categories
```

The junction table:

```text
product_categories
```

should contain:

- `product_id`
- `category_id`

Recommended primary key:

```text
(product_id, category_id)
```

---

# 11. Products and Inventory

Relationship:

```text
products 1 ─────── N inventory
```

A product may have one or multiple inventory records depending on:

- Warehouse
- Location
- Seller
- Variant

Recommended relationship:

```text
products.id
    ↓
inventory.product_id
```

If product variants are supported, inventory should also reference:

```text
variant_id
```

---

# 12. Products and Order Items

Relationship:

```text
products 1 ─────── N order_items
```

A product may appear in many orders.

Recommended relationship:

```text
products.id
    ↓
order_items.product_id
```

The order item must also preserve historical product information through snapshot fields.

Examples:

```text
product_name_snapshot
sku_snapshot
variant_name_snapshot
unit_price
```

---

# 13. Products and Reviews

Relationship:

```text
products 1 ─────── N reviews
```

A product may receive multiple customer reviews.

Recommended relationship:

```text
products.id
    ↓
reviews.product_id
```

Only eligible published reviews should contribute to the public rating calculation.

---

# 14. Products and Coupons

Relationship:

```text
products N ─────── N coupons
```

A coupon may apply to:

- One Product
- Multiple Products

A product may be eligible for:

- One Coupon
- Multiple Coupons

Recommended implementation:

```text
coupons
    │
    ▼
coupon_products
    ▲
    │
products
```

The junction table should contain:

- `coupon_id`
- `product_id`

---

# 15. Orders and Order Items

Relationship:

```text
orders 1 ─────── N order_items
```

An order must contain one or more order items.

Recommended relationship:

```text
orders.id
    ↓
order_items.order_id
```

Example:

```text
Order
│
├── Item 1
├── Item 2
└── Item 3
```

---

# 16. Orders and Payments

Relationship:

```text
orders 1 ─────── N payments
```

An order may have multiple payment attempts.

Example:

```text
Order
│
├── Payment Attempt 1 → Failed
├── Payment Attempt 2 → Failed
└── Payment Attempt 3 → Paid
```

Recommended relationship:

```text
orders.id
    ↓
payments.order_id
```

---

# 17. Orders and Shipping

Relationship:

```text
orders 1 ─────── N shipping
```

An order may contain one or multiple shipments.

This supports:

- Split Shipments
- Multi-Seller Orders
- Partial Fulfillment

Recommended relationship:

```text
orders.id
    ↓
shipping.order_id
```

---

# 18. Orders and Coupons

Relationship:

```text
coupons 1 ─────── N orders
```

A coupon may be used by multiple orders.

Recommended relationship:

```text
coupons.id
    ↓
orders.coupon_id
```

For multiple coupons per order, a dedicated junction table should be introduced.

---

# 19. Orders and Reviews

Relationship:

```text
orders 1 ─────── N reviews
```

An order may be associated with multiple reviews.

Reviews may reference:

- Order
- Order Item

Recommended relationships:

```text
orders.id
    ↓
reviews.order_id
```

and:

```text
order_items.id
    ↓
reviews.order_item_id
```

This supports verified purchase validation.

---

# 20. Order Items and Reviews

Relationship:

```text
order_items 1 ─────── N reviews
```

Depending on the review policy, one order item may receive:

- One Review
- Multiple Reviews

The recommended initial rule is:

```text
One Customer
+
One Order Item
=
One Product Review
```

The exact database uniqueness constraint should be finalized before implementation.

---

# 21. Coupons and Coupon Usage

Relationship:

```text
coupons 1 ─────── N coupon_usages
```

A coupon may be used multiple times.

Recommended relationship:

```text
coupons.id
    ↓
coupon_usages.coupon_id
```

Coupon usage should also reference:

```text
user_id
order_id
```

This allows usage tracking by customer and order.

---

# 22. Orders and Seller Orders

For a multi-vendor marketplace, the recommended architecture is:

```text
Customer Order
       │
       ├── Seller Order A
       │      ├── Order Item
       │      └── Order Item
       │
       └── Seller Order B
              ├── Order Item
              └── Order Item
```

Recommended tables:

```text
orders
seller_orders
order_items
```

Relationships:

```text
orders 1 ─────── N seller_orders
```

and:

```text
seller_orders 1 ─────── N order_items
```

This structure is recommended for Maroc Local Hub because the platform is designed as a multi-vendor marketplace.

---

# 23. Seller Orders and Shipping

Relationship:

```text
seller_orders 1 ─────── N shipping
```

This allows each seller to manage independent fulfillment.

Example:

```text
Customer Order
│
├── Seller Order A
│      └── Shipment A
│
└── Seller Order B
       └── Shipment B
```

---

# 24. Seller Orders and Payments

Customer payments are associated with the parent order.

Seller financial allocation should be handled separately.

Recommended future structure:

```text
orders
    │
    └── payments
          │
          ▼
    financial allocation
          │
          ├── seller revenue
          ├── platform commission
          └── payment fees
```

Future tables may include:

- `seller_balances`
- `seller_payouts`
- `platform_commissions`
- `payout_transactions`

---

# 25. Payment and Refund Relationships

Recommended structure:

```text
payments 1 ─────── N refunds
```

A payment may have:

- No Refund
- One Full Refund
- Multiple Partial Refunds

Recommended relationship:

```text
payments.id
    ↓
refunds.payment_id
```

Refunds should not overwrite the original payment record.

---

# 26. Shipping and Shipping Events

Relationship:

```text
shipping 1 ─────── N shipping_events
```

Each shipment may have multiple tracking events.

Example:

```text
Shipment Created
      ↓
Picked Up
      ↓
In Transit
      ↓
Out for Delivery
      ↓
Delivered
```

Recommended relationship:

```text
shipping.id
    ↓
shipping_events.shipping_id
```

---

# 27. Reviews and Review Images

Relationship:

```text
reviews 1 ─────── N review_images
```

A review may contain multiple images.

Recommended relationship:

```text
reviews.id
    ↓
review_images.review_id
```

Images should be stored externally or through an object storage system.

The database should store references rather than binary image data where possible.

---

# 28. Reviews and Seller Replies

Relationship:

```text
reviews 1 ─────── N review_replies
```

A review may receive one or more replies depending on the final business rules.

Seller replies must remain separate from customer reviews.

---

# 29. Users and Coupon Usage

Relationship:

```text
users 1 ─────── N coupon_usages
```

A customer may use multiple coupons across different orders.

Recommended relationship:

```text
users.id
    ↓
coupon_usages.user_id
```

This relationship supports per-user coupon limits.

---

# 30.
