# Payments API

Document ID: MLH-API-010

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Payments API for Maroc Local Hub.

It specifies all endpoints related to payment processing, transaction management, refunds, payment verification, and financial records.

---

# Objectives

The Payments API should:

- Process secure payments
- Support multiple payment methods
- Verify payment status
- Handle refunds
- Maintain transaction history
- Integrate with payment gateways

---

# Base URL

```
/api/v1/payments
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| POST | /checkout | Create payment session |
| POST | /confirm | Confirm payment |
| GET | /{id} | Get payment details |
| GET | /transactions | Get transaction history |
| POST | /refund | Request refund |
| GET | /methods | Get available payment methods |
| POST | /webhook | Payment gateway callback |

---

# Checkout

### Endpoint

```
POST /api/v1/payments/checkout
```

### Request Body

```json
{
  "orderId": 1001,
  "paymentMethod": "Credit Card"
}
```

### Success Response

```json
{
  "success": true,
  "paymentId": 501,
  "paymentUrl": "https://payment.example.com/session/abc123"
}
```

---

# Confirm Payment

### Endpoint

```
POST /api/v1/payments/confirm
```

### Purpose

Confirm the payment after receiving a successful response from the payment provider.

---

# Get Payment Details

### Endpoint

```
GET /api/v1/payments/{id}
```

Returns:

- Payment ID
- Order ID
- Amount
- Currency
- Payment Method
- Status
- Transaction Reference
- Payment Date

---

# Transaction History

### Endpoint

```
GET /api/v1/payments/transactions
```

Supports:

- Pagination
- Date filtering
- Status filtering
- Payment method filtering

---

# Refund

### Endpoint

```
POST /api/v1/payments/refund
```

### Request Body

```json
{
  "paymentId": 501,
  "reason": "Customer request"
}
```

---

# Payment Methods

### Endpoint

```
GET /api/v1/payments/methods
```

Supported methods may include:

- Credit Card
- Debit Card
- Bank Transfer
- Cash on Delivery
- Mobile Wallet

Future integrations may include:

- Stripe
- PayPal
- PayTabs
- CMI
- Payzone

---

# Payment Status

Supported values:

- Pending
- Authorized
- Paid
- Failed
- Cancelled
- Refunded
- Partially Refunded

---

# Webhook

### Endpoint

```
POST /api/v1/payments/webhook
```

Purpose:

Receive secure notifications from payment providers about transaction status changes.

---

# Validation Rules

Examples include:

- Order must exist
- Payment amount must match order total
- Supported payment method is required
- Refund amount cannot exceed the original payment

---

# Authentication

Protected endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

Webhook endpoints should use provider-specific signature verification.

---

# Error Responses

Possible errors include:

- PAYMENT_NOT_FOUND
- PAYMENT_FAILED
- INVALID_PAYMENT_METHOD
- REFUND_NOT_ALLOWED
- INVALID_TRANSACTION
- VALIDATION_ERROR
- UNAUTHORIZED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Secure Payment Gateway Integration
- Webhook Signature Verification
- Audit Logging
- PCI DSS Compliance (where applicable)

---

# Future Enhancements

Future improvements may include:

- Split Payments
- Escrow Payments
- Installment Payments
- Subscription Billing
- Multi-currency Payments
- AI Fraud Detection

---

# Related Documents

- orders-api.md
- shipping-api.md
- ../09-database/payments-table.md

---

# Conclusion

The Payments API provides a secure, reliable, and scalable payment infrastructure, enabling seamless financial transactions while protecting customer data and supporting future payment integrations.

---

End of Document
