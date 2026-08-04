# Cart API

Document ID: MLH-API-008

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Cart API for Maroc Local Hub.

It specifies all endpoints related to shopping cart management, including adding products, updating quantities, removing items, calculating totals, and preparing for checkout.

---

# Objectives

The Cart API should:

- Create and manage shopping carts
- Add and remove products
- Update item quantities
- Calculate totals
- Validate product availability
- Prepare carts for checkout

---

# Base URL

```
/api/v1/cart
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get current cart |
| POST | /items | Add item to cart |
| PUT | /items/{itemId} | Update cart item quantity |
| DELETE | /items/{itemId} | Remove item from cart |
| DELETE | /clear | Clear cart |
| GET | /summary | Get cart summary |
| POST | /validate | Validate cart before checkout |

---

# Get Current Cart

### Endpoint

```
GET /api/v1/cart
```

### Authentication

Bearer Token Required.

### Response

Returns:

- Cart ID
- User ID
- Items
- Quantity
- Subtotal
- Discount
- Taxes
- Shipping Estimate
- Total

---

# Add Item

### Endpoint

```
POST /api/v1/cart/items
```

### Request Body

```json
{
  "productId": 15,
  "quantity": 2
}
```

### Success Response

```json
{
  "success": true,
  "message": "Item added to cart."
}
```

---

# Update Item Quantity

### Endpoint

```
PUT /api/v1/cart/items/{itemId}
```

### Request Body

```json
{
  "quantity": 3
}
```

---

# Remove Item

### Endpoint

```
DELETE /api/v1/cart/items/{itemId}
```

Removes the selected item from the cart.

---

# Clear Cart

### Endpoint

```
DELETE /api/v1/cart/clear
```

Removes all items from the shopping cart.

---

# Cart Summary

### Endpoint

```
GET /api/v1/cart/summary
```

Returns:

- Item Count
- Subtotal
- Discounts
- Estimated Shipping
- Taxes
- Grand Total

---

# Validate Cart

### Endpoint

```
POST /api/v1/cart/validate
```

Validation includes:

- Product availability
- Stock quantity
- Product status
- Price changes
- Seller availability

---

# Validation Rules

Examples include:

- Product must exist
- Quantity must be greater than zero
- Quantity cannot exceed available stock
- Product must be active

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Error Responses

Possible errors include:

- CART_NOT_FOUND
- PRODUCT_NOT_FOUND
- OUT_OF_STOCK
- INVALID_QUANTITY
- PRODUCT_UNAVAILABLE
- VALIDATION_ERROR
- UNAUTHORIZED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Input Validation
- Rate Limiting
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- Saved carts
- Guest carts
- Shared carts
- Wishlist integration
- AI product recommendations
- Automatic coupon suggestions

---

# Related Documents

- products-api.md
- inventory-api.md
- orders-api.md

---

# Conclusion

The Cart API provides a reliable shopping cart management system that supports accurate pricing, inventory validation, and a smooth checkout experience.

---

End of Document
