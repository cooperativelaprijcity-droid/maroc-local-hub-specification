# Inventory API

Document ID: MLH-API-007

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Inventory API for Maroc Local Hub.

It specifies all endpoints related to inventory management, stock levels, stock movements, availability, reservations, and inventory tracking.

---

# Objectives

The Inventory API should:

- Track product inventory
- Update stock quantities
- Prevent overselling
- Support inventory reservations
- Maintain inventory history
- Provide real-time stock availability

---

# Base URL

```
/api/v1/inventory
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get inventory records |
| GET | /{productId} | Get inventory for a product |
| POST | / | Create inventory record |
| PUT | /{productId} | Update inventory |
| PATCH | /{productId}/adjust | Adjust stock quantity |
| POST | /{productId}/reserve | Reserve stock |
| POST | /{productId}/release | Release reserved stock |
| GET | /movements | Get inventory movement history |
| GET | /low-stock | Get low stock products |

---

# Get Inventory

### Endpoint

```
GET /api/v1/inventory
```

Supports:

- Pagination
- Product filtering
- Seller filtering
- Stock status filtering

---

# Get Product Inventory

### Endpoint

```
GET /api/v1/inventory/{productId}
```

### Response

Returns:

- Product ID
- Available Quantity
- Reserved Quantity
- Total Quantity
- Stock Status
- Last Updated

---

# Create Inventory Record

### Endpoint

```
POST /api/v1/inventory
```

### Request Body

```json
{
  "productId": 15,
  "quantity": 250
}
```

---

# Update Inventory

### Endpoint

```
PUT /api/v1/inventory/{productId}
```

Allows updating the available stock quantity.

---

# Adjust Stock

### Endpoint

```
PATCH /api/v1/inventory/{productId}/adjust
```

### Request Body

```json
{
  "adjustment": -5,
  "reason": "Damaged items"
}
```

Reasons may include:

- Sale
- Return
- Damage
- Manual Correction
- Inventory Audit

---

# Reserve Stock

### Endpoint

```
POST /api/v1/inventory/{productId}/reserve
```

Purpose:

Reserve inventory during checkout before payment confirmation.

---

# Release Reserved Stock

### Endpoint

```
POST /api/v1/inventory/{productId}/release
```

Purpose:

Release previously reserved inventory if an order is cancelled or expires.

---

# Inventory Movements

### Endpoint

```
GET /api/v1/inventory/movements
```

Returns inventory history including:

- Date
- Product
- Quantity Change
- Reason
- User
- Reference

---

# Low Stock

### Endpoint

```
GET /api/v1/inventory/low-stock
```

Returns products below the configured stock threshold.

---

# Stock Status

Supported values:

- In Stock
- Low Stock
- Out of Stock
- Reserved

---

# Validation Rules

Examples include:

- Product must exist
- Quantity cannot be negative
- Adjustment reason is required
- Reservation cannot exceed available quantity

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

Inventory management is restricted to authorized sellers and administrators.

---

# Error Responses

Possible errors include:

- PRODUCT_NOT_FOUND
- INVENTORY_NOT_FOUND
- INSUFFICIENT_STOCK
- INVALID_QUANTITY
- VALIDATION_ERROR
- UNAUTHORIZED
- FORBIDDEN

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- Audit Logging
- Input Validation

---

# Future Enhancements

Future improvements may include:

- Multi-warehouse inventory
- Warehouse transfers
- Barcode integration
- QR code support
- Automated stock alerts
- Demand forecasting

---

# Related Documents

- products-api.md
- sellers-api.md
- ../09-database/inventory-table.md

---

# Conclusion

The Inventory API provides accurate and secure inventory management, helping sellers maintain product availability while supporting efficient marketplace operations.

---

End of Document
