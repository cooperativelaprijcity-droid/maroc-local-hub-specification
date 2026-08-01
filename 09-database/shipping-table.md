# Shipping Table Specification

Document ID: MLH-DB-010
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `shipping` table used to manage shipment information for orders placed on Maroc Local Hub.

The shipping system must support delivery addresses, shipping providers, tracking information, shipment status, delivery methods, estimated delivery dates, and multi-seller fulfillment.

---

# Table Name

`shipping`

---

# Description

The `shipping` table stores shipment records associated with customer orders.

Shipping information is separated from the `orders` table because one order may contain:

- One Shipment
- Multiple Shipments
- Multiple Sellers
- Multiple Delivery Providers
- Partial Fulfillment
- Split Deliveries

This structure allows Maroc Local Hub to support a scalable multi-vendor marketplace.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique shipment identifier |
| `order_id` | UUID | Yes | Foreign Key, Not Null | Parent order |
| `seller_id` | UUID | No | Foreign Key, Nullable | Seller responsible for shipment |
| `shipping_provider` | VARCHAR(100) | Yes | Not Null | Delivery company or provider |
| `shipping_method` | VARCHAR(50) | Yes | Not Null | Delivery method |
| `tracking_number` | VARCHAR(255) | No | Nullable | Shipment tracking number |
| `status` | VARCHAR(40) | Yes | Not Null | Current shipment status |
| `recipient_name` | VARCHAR(255) | Yes | Not Null | Recipient full name |
| `recipient_phone` | VARCHAR(30) | Yes | Not Null | Recipient phone number |
| `country_code` | VARCHAR(10) | Yes | Not Null | Destination country |
| `region` | VARCHAR(150) | No | Nullable | Destination region |
| `city` | VARCHAR(150) | Yes | Not Null | Destination city |
| `postal_code` | VARCHAR(30) | No | Nullable | Postal or ZIP code |
| `address_line_1` | TEXT | Yes | Not Null | Main delivery address |
| `address_line_2` | TEXT | No | Nullable | Additional address information |
| `delivery_instructions` | TEXT | No | Nullable | Customer delivery instructions |
| `shipping_cost` | DECIMAL(12,2) | Yes | Default 0 | Shipping cost |
| `currency_code` | VARCHAR(10) | Yes | Not Null | Shipping cost currency |
| `estimated_delivery_at` | TIMESTAMP | No | Nullable | Estimated delivery time |
| `shipped_at` | TIMESTAMP | No | Nullable | Shipment dispatch timestamp |
| `delivered_at` | TIMESTAMP | No | Nullable | Delivery completion timestamp |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Last update timestamp |

---

# Shipping Status

Supported values should include:

- `pending`
- `processing`
- `ready_for_pickup`
- `picked_up`
- `in_transit`
- `out_for_delivery`
- `delivered`
- `failed_delivery`
- `returned`
- `cancelled`

The final status list should align with the selected shipping providers.

---

# Shipping Methods

Supported methods may include:

- `standard`
- `express`
- `same_day`
- `pickup_point`
- `seller_pickup`
- `cash_on_delivery`

Additional methods may be introduced according to marketplace requirements.

---

# Shipping Provider

The `shipping_provider` field identifies the delivery company or logistics provider responsible for the shipment.

Examples may include:

- Local Delivery Company
- National Courier
- International Courier
- Marketplace Logistics
- Seller-Managed Delivery

The platform should maintain a dedicated provider configuration system in future versions.

---

# Tracking Number

The `tracking_number` stores the tracking reference provided by the shipping provider.

The tracking number should be:

- Unique where supported by the provider.
- Stored securely.
- Visible to authorized customers.
- Visible to the relevant seller.
- Accessible to administrators.

The platform may later support direct tracking links.

---

# Delivery Address

The shipment should preserve the delivery address used at the time the order was placed.

The system should not rely exclusively on the customer's current profile address.

Recommended approach:

Store an immutable address snapshot in the shipment record or in a dedicated:

`order_addresses`

table.

This ensures historical accuracy when the customer changes their address after placing an order.

---

# Recipient Information

The shipment should preserve:

- Recipient Name
- Recipient Phone Number
- Delivery Address

These values should represent the information used for the actual shipment.

---

# Seller Relationship

In a multi-vendor marketplace, different products in one customer order may be fulfilled by different sellers.

Therefore, a shipment may optionally reference:

`seller_id`

Example:

```text
Customer Order
│
├── Shipment A
│     └── Seller A
│
└── Shipment B
      └── Seller B
```

This allows the platform to support split shipments.

---

# Multi-Seller Orders

A customer may purchase products from multiple sellers in a single checkout.

The recommended structure is:

```text
Customer Order
      │
      ├── Seller Order A
      │       └── Shipment A
      │
      └── Seller Order B
              └── Shipment B
```

This structure should be coordinated with:

- `orders`
- `seller_orders`
- `order_items`
- `shipping`

The final architecture should be confirmed before production implementation.

---

# Shipping Cost

The `shipping_cost` field stores the cost charged for the shipment.

Shipping costs may be calculated based on:

- Destination
- Weight
- Dimensions
- Seller
- Delivery Method
- Shipping Provider
- Order Value
- Promotional Rules

Historical shipping costs must remain unchanged after the order is completed.

---

# Currency

The `currency_code` stores the currency associated with the shipping cost.

Examples:

- `MAD`
- `EUR`
- `USD`

Historical shipment records must preserve the original currency.

---

# Estimated Delivery

The `estimated_delivery_at` field represents the expected delivery date or time.

This value may be updated when:

- Shipping provider changes the estimate.
- Shipment is delayed.
- Customer changes delivery method.

Changes to delivery estimates may be recorded in shipment history.

---

# Shipping Events

A dedicated table is recommended:

`shipping_events`

This table should record shipment tracking history.

Possible fields:

- `id`
- `shipping_id`
- `status`
- `location`
- `description`
- `event_timestamp`
- `created_at`

Example:

```text
Shipment Created
      ↓
Picked Up
      ↓
In Transit
      ↓
Arrived at Local Facility
      ↓
Out for Delivery
      ↓
Delivered
```

This provides a complete tracking history.

---

# Delivery Confirmation

When a shipment is delivered, the platform may record:

- Delivery Timestamp
- Recipient Confirmation
- Delivery Proof
- Signature
- Photo
- OTP Verification

Future implementations may introduce:

`delivery_confirmations`

for detailed proof-of-delivery management.

---

# Failed Delivery

If delivery fails, the system should record:

- Failure Reason
- Attempt Number
- Delivery Timestamp
- Next Attempt Date

Possible reasons include:

- Recipient Unavailable
- Incorrect Address
- Phone Unreachable
- Refused Delivery
- Damaged Package

The exact reason should be stored in a dedicated event or status history record.

---

# Returns

Returned shipments should be tracked separately from the original shipment when possible.

Future support may include:

- Return Shipment
- Return Tracking Number
- Return Provider
- Return Reason
- Return Status

A dedicated table may be introduced:

`return_shipments`

---

# Cash on Delivery

If Cash on Delivery is supported, shipping and payment workflows must be coordinated.

Typical workflow:

```text
Order Created
      ↓
Cash on Delivery Selected
      ↓
Shipment Created
      ↓
Shipment Delivered
      ↓
Cash Collected
      ↓
Payment Confirmed
```

The payment system must record the final payment status.

---

# Shipping Providers

Future versions may support a dedicated table:

`shipping_providers`

Possible fields:

- Provider ID
- Provider Name
- Country
- API Endpoint
- Integration Status
- Tracking URL Template
- Created At
- Updated At

API credentials must never be stored in plain text.

Secrets should be stored using secure secret management systems.

---

# Indexes

The following indexes are recommended:

- Index on `order_id`
- Index on `seller_id`
- Index on `tracking_number`
- Index on `status`
- Index on `shipping_provider`
- Index on `city`
- Index on `country_code`
-
