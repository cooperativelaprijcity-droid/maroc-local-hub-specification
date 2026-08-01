# Payments Table Specification

Document ID: MLH-DB-009
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `payments` table used to record payment transactions associated with orders on Maroc Local Hub.

The payment system must support secure transaction tracking, multiple payment attempts, payment provider integration, refunds, partial refunds, and payment status management.

---

# Table Name

`payments`

---

# Description

The `payments` table stores payment transaction records related to customer orders.

Payment data must remain separate from the `orders` table because one order may have:

- Multiple Payment Attempts
- Failed Payments
- Successful Payments
- Partial Payments
- Refunds
- Partial Refunds

The table should store transaction references and payment status without storing sensitive card information.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique payment identifier |
| `order_id` | UUID | Yes | Foreign Key, Not Null | Associated order |
| `user_id` | UUID | Yes | Foreign Key, Not Null | Customer who initiated payment |
| `payment_provider` | VARCHAR(50) | Yes | Not Null | Payment service provider |
| `provider_transaction_id` | VARCHAR(255) | No | Nullable | Transaction ID provided by payment provider |
| `payment_method` | VARCHAR(50) | Yes | Not Null | Payment method used |
| `amount` | DECIMAL(12,2) | Yes | Not Null | Payment amount |
| `currency_code` | VARCHAR(10) | Yes | Not Null | Currency used |
| `status` | VARCHAR(40) | Yes | Not Null | Current payment status |
| `failure_code` | VARCHAR(100) | No | Nullable | Provider failure code |
| `failure_message` | TEXT | No | Nullable | Payment failure description |
| `metadata` | JSON | No | Nullable | Additional provider metadata |
| `processed_at` | TIMESTAMP | No | Nullable | Successful processing timestamp |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Last update timestamp |

---

# Payment Providers

The platform should support integration with external payment providers.

Possible providers may include:

- Stripe
- PayPal
- Local Moroccan Payment Providers
- Bank Transfer
- Cash on Delivery

The final list depends on the payment architecture and supported countries.

Payment provider credentials and secret keys must never be stored in this table.

---

# Payment Methods

Supported payment methods may include:

- `credit_card`
- `debit_card`
- `paypal`
- `bank_transfer`
- `cash_on_delivery`
- `mobile_payment`

The final supported methods should be defined by the platform's payment integrations.

---

# Payment Status

Supported values should include:

- `pending`
- `processing`
- `authorized`
- `paid`
- `failed`
- `cancelled`
- `refunded`
- `partially_refunded`

Payment status must be independent from order status.

---

# Payment Lifecycle

A typical payment lifecycle is:

```text
Payment Created
      ↓
Pending
      ↓
Processing
      ↓
Authorized
      ↓
Paid
```

Possible failure path:

```text
Pending
   ↓
Processing
   ↓
Failed
```

Possible refund path:

```text
Paid
   ↓
Refund Requested
   ↓
Refunded
```

---

# Order Relationship

Each payment belongs to an order.

Recommended relationship:

`orders.id → payments.order_id`

An order may have multiple payment records.

Example:

```text
Order
│
├── Payment Attempt 1 → Failed
│
├── Payment Attempt 2 → Failed
│
└── Payment Attempt 3 → Paid
```

The system must identify the successful payment transaction.

---

# User Relationship

Each payment is associated with the customer who initiated the transaction.

Recommended relationship:

`users.id → payments.user_id`

Historical payment records should remain available even if the user later closes their account.

---

# Provider Transaction ID

The `provider_transaction_id` stores the external payment provider's transaction identifier.

Examples include:

- Payment Intent ID
- Transaction ID
- Charge ID
- Authorization ID

This field should be indexed for reconciliation and webhook processing.

The uniqueness constraint should depend on the payment provider.

Recommended unique combination:

```text
payment_provider + provider_transaction_id
```

---

# Amount and Currency

Payment amounts must use precise decimal arithmetic.

The system must not use floating-point values for financial calculations.

The `currency_code` must represent the currency used for the transaction.

Examples:

- `MAD`
- `EUR`
- `USD`

Historical payment currency must never be changed.

---

# Refunds

Refunds should not overwrite the original payment amount.

Instead, refunds should be recorded separately.

Recommended table:

`refunds`

A refund record may contain:

- Refund ID
- Payment ID
- Order ID
- Amount
- Currency
- Provider Refund ID
- Status
- Reason
- Created At
- Processed At

This allows support for:

- Full Refunds
- Partial Refunds
- Multiple Refunds

---

# Payment Attempts

Each payment attempt should create a separate payment record when appropriate.

Example:

```text
Order #MLH-2026-000001

Attempt 1
Status: Failed

Attempt 2
Status: Failed

Attempt 3
Status: Paid
```

This provides a complete history of payment attempts.

---

# Webhooks

Payment providers may send asynchronous webhook events.

The payment system should support webhook processing for events such as:

- Payment Succeeded
- Payment Failed
- Payment Authorized
- Payment Cancelled
- Refund Succeeded
- Refund Failed

Webhook events should be processed idempotently.

A dedicated table is recommended:

`payment_webhook_events`

This table should store:

- Event ID
- Provider
- Provider Event ID
- Event Type
- Payload Reference
- Processing Status
- Received At
- Processed At

---

# Security Requirements

The platform must never store:

- Full Credit Card Numbers
- CVV Codes
- Card PINs
- Payment Provider Secret Keys

Sensitive payment data must be handled by compliant payment providers.

The platform should store only safe references and transaction identifiers.

---

# PCI Considerations

When using card payments, the platform should minimize the scope of PCI DSS compliance by using hosted checkout pages or tokenized payment methods where possible.

Payment card data should be processed by the payment provider.

---

# Idempotency

Payment operations must support idempotency.

This prevents duplicate charges caused by:

- Network Retries
- Duplicate Requests
- Webhook Replays
- Client Retries

The payment system should use an idempotency key where supported by the payment provider.

---

# Database Indexes

The following indexes are recommended:

- Index on `order_id`
- Index on `user_id`
- Index on `payment_provider`
- Index on `provider_transaction_id`
- Index on `status`
- Index on `created_at`

A unique composite index may be used for:

```text
payment_provider + provider_transaction_id
```

when the provider guarantees transaction ID uniqueness.

---

# Constraints

The table should enforce:

- `order_id` must reference a valid order.
- `user_id` must reference a valid user.
- `amount` must be greater than zero.
- `currency_code` must be valid.
- `status` must contain a supported value.
- Provider transaction references must be validated.
- Sensitive payment information must not be stored.

---

# Payment Reconciliation

The platform should support payment reconciliation between:

- Internal Payment Records
- Orders
- Payment Providers
- Bank Statements
- Seller Payouts

Reconciliation processes should identify:

- Missing Payments
- Duplicate Payments
- Failed Transactions
- Unmatched Transactions
- Refund Differences

---

# Seller Marketplace Payments

In a multi-vendor marketplace, customer payments may need to be allocated between:

- Platform Commission
- Seller Revenue
- Payment Processing Fees
- Taxes
- Refunds

The `payments` table should not attempt to store the complete seller accounting model.

A dedicated financial or payout system should manage:

- Seller Balances
- Platform Commission
- Seller Payouts
- Payout Status
- Payout Transactions

Recommended future tables include:

- `seller_payouts`
- `seller_balances`
- `platform_commissions`
- `payout_transactions`

---

# Cash on Delivery

If Cash on Delivery is supported, payment records may be created with:

```text
payment_method = cash_on_delivery
```

The payment status should remain pending until the delivery is successfully collected.

Possible flow:

```text
Order Created
      ↓
Cash on Delivery
      ↓
Order Shipped
      ↓
Order Delivered
      ↓
Cash Collected
      ↓
Payment Marked as Paid
```

The exact workflow should be aligned with the shipping and fulfillment system.

---

# Data Lifecycle

Payment records should follow a lifecycle such as:

1. Payment initiated.
2. Payment processing.
3. Payment authorized.
4. Payment completed.
5. Payment may be refunded.

Failed payment attempts should remain available for auditing.

Payment records should not be deleted casually.

---

# Performance Notes

The `payments` table may be queried for:

- Customer Payment History
- Order Payment Status
- Payment Reconciliation
- Refund Processing
- Financial Reporting
- Fraud Detection

Indexes should be optimized according to actual production query patterns.

---

# Auditability

Payment-related actions should be auditable.

The platform should maintain records of:

- Payment Status Changes
- Refund Requests
- Refund Approvals
- Administrative Adjustments
- Reconciliation Actions

A dedicated payment audit log may be introduced.

---

# Future Enhancements

Future versions may support:

- Payment Splitting
- Marketplace Payment Escrow
- Seller Payout Automation
- Subscription Billing
- Recurring Payments
- Installment Payments
- Buy Now Pay Later
- Fraud Detection
- AI Payment Risk Analysis
- Multi-Currency Settlement

---

# Related Documents

Related database specifications include:

- `orders-table.md`
- `order-items-table.md`
- `users-table.md`
- `sellers-table.md`
- `shipping-table.md`
- `database-relationships.md`

---

# Conclusion

The `payments` table provides the transaction-level payment record for Maroc Local Hub.

The architecture must separate payment transactions from orders, refunds, seller payouts, and provider webhook events.

The system must prioritize security, idempotency, auditability, financial accuracy, and compliance while remaining flexible enough to support a multi-vendor marketplace.

---

End of Document
