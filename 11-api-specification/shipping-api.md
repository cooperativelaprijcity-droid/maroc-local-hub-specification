# Shipping API

Document ID: MLH-API-011

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Shipping API for Maroc Local Hub.

It specifies all endpoints related to shipping methods, shipping rates, shipment creation, tracking, delivery status, and logistics integration.

---

# Objectives

The Shipping API should:

- Manage shipping methods
- Calculate shipping costs
- Create shipments
- Track deliveries
- Support multiple logistics providers
- Improve delivery visibility

---

# Base URL

```
/api/v1/shipping
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /methods | Get available shipping methods |
| POST | /calculate | Calculate shipping cost |
| POST | /shipments | Create shipment |
| GET | /shipments/{id} | Get shipment details |
| GET | /tracking/{trackingNumber} | Track shipment |
| PATCH | /shipments/{id}/status | Update shipment status |
| GET | /carriers | Get supported carriers |

---

# Get Shipping Methods

### Endpoint

```
GET /api/v1/shipping/methods
```

Returns available shipping options such as:

- Standard Delivery
- Express Delivery
- Same-Day Delivery (where available)
- Store Pickup

---

# Calculate Shipping Cost

### Endpoint

```
POST /api/v1/shipping/calculate
```

### Request Body

```json
{
  "destinationCountry": "Morocco",
  "destinationCity": "Guelmim",
  "weight": 2.5,
  "shippingMethod": "Standard"
}
```

### Success Response

```json
{
  "shippingCost": 45,
  "currency": "MAD",
  "estimatedDeliveryDays": 3
}
```

---

# Create Shipment

### Endpoint

```
POST /api/v1/shipping/shipments
```

### Request Body

```json
{
  "orderId": 1001,
  "carrier": "Amana",
  "shippingMethod": "Standard"
}
```

---

# Shipment Details

### Endpoint

```
GET /api/v1/shipping/shipments/{id}
```

Returns:

- Shipment ID
- Order ID
- Carrier
- Tracking Number
- Shipping Method
- Status
- Estimated Delivery
- Delivery Address

---

# Shipment Tracking

### Endpoint

```
GET /api/v1/shipping/tracking/{trackingNumber}
```

Returns:

- Tracking Number
- Current Status
- Current Location
- Shipment Timeline
- Estimated Delivery Date

---

# Shipment Status

Supported values:

- Pending
- Ready for Pickup
- Picked Up
- In Transit
- Out for Delivery
- Delivered
- Delivery Failed
- Returned

---

# Supported Carriers

Examples include:

- Amana
- Chronopost
- DHL
- Aramex
- FedEx

Additional logistics partners may be integrated in future releases.

---

# Validation Rules

Examples include:

- Order must exist
- Shipping address is required
- Supported carrier is required
- Package weight must be greater than zero

---

# Authentication

Protected endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Error Responses

Possible errors include:

- SHIPMENT_NOT_FOUND
- INVALID_ADDRESS
- UNSUPPORTED_CARRIER
- INVALID_SHIPPING_METHOD
- VALIDATION_ERROR
- UNAUTHORIZED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- Input Validation
- Audit Logging

---

# Future
