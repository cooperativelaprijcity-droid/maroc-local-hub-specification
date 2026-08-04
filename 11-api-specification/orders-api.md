# Orders API

Document ID: MLH-API-009

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Orders API for Maroc Local Hub.

It specifies all endpoints related to order creation, order management, payment status, fulfillment, shipping, cancellation, returns, and order tracking.

---

# Objectives

The Orders API should:

- Create customer orders
- Manage order lifecycle
- Support order tracking
- Handle cancellations and returns
- Integrate with payment and shipping services
- Maintain complete order history

---

# Base URL

```
/api/v1/orders
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get user orders |
| GET | /{id} | Get order details |
| POST | / | Create new order |
| PATCH | /{id}/cancel | Cancel order |
| PATCH | /{id}/status | Update order status |
| POST | /{id}/return | Request product return |
| GET | /{id}/tracking | Track shipment |
| GET | /statistics | Get order statistics |

---

# Create Order

### Endpoint

```
POST /api/v1/orders
```

### Request Body

```json
{
  "cartId": 25,
  "shippingAddressId": 7,
  "paymentMethod": "Credit Card",
  "shippingMethod": "Standard"
}
```

### Success Response

```json
{
  "success": true,
  "orderId": 1001,
  "message": "Order created successfully."
}
```

---

# Get Orders

### Endpoint

```
GET /api/v1/orders
```

Supports:

- Pagination
- Date filtering
- Status filtering
- Sorting

---

# Get Order Details

### Endpoint

```
GET /api/v1/orders/{id}
```

Returns:

- Order Number
- Customer
- Products
- Seller
- Shipping Address
- Payment Method
- Shipping Method
- Status
- Tracking Number
- Total Amount
- Order Date

---

# Order Status

Supported values:

- Pending
- Confirmed
- Processing
- Packed
- Shipped
- Delivered
- Cancelled
- Returned
- Refunded

---

# Cancel Order

### Endpoint

```
PATCH /api/v1/orders/{id}/cancel
```

Cancellation rules may depend on:

- Current order status
- Payment status
- Shipping status

---

# Return Request

### Endpoint

```
POST /api/v1/orders/{id}/return
```

### Request Body

```json
{
  "reason": "Damaged product"
}
```

---

# Shipment Tracking

### Endpoint

```
GET /api/v1/orders/{id}/tracking
```

Returns:

- Carrier
- Tracking Number
- Shipment Status
- Estimated Delivery Date

---

# Order Statistics

### Endpoint

```
GET /api/v1/orders/statistics
```

Returns:

- Total Orders
- Completed Orders
- Cancelled Orders
- Returned Orders
- Revenue
- Average Order Value

---

# Validation Rules

Examples include:

- Cart must not be empty
- Shipping address is required
- Payment method must be supported
- Products must be in stock

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Error Responses

Possible errors include:

- ORDER_NOT_FOUND
- CART_EMPTY
- PAYMENT_REQUIRED
- OUT_OF_STOCK
- INVALID_STATUS
- RETURN_NOT_ALLOWED
- VALIDATION_ERROR
- UNAUTHORIZED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- Order Ownership Validation
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- Split Orders
- Partial Returns
- Scheduled Deliveries
- Gift Orders
- Subscription Orders
- AI Delivery Predictions

---

# Related Documents

- cart-api.md
- payments-api.md
- shipping-api.md
- ../09-database/orders-table.md

---

# Conclusion

The Orders API provides a secure and scalable framework for managing the complete order lifecycle, ensuring accurate processing, reliable tracking, and an excellent customer experience.

---

End of Document
