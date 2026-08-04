# Products API

Document ID: MLH-API-005

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Products API for Maroc Local Hub.

It specifies all endpoints related to product creation, management, search, filtering, inventory visibility, publishing, and product lifecycle management.

---

# Objectives

The Products API should:

- Create and manage products
- Retrieve product details
- Support advanced search
- Support filtering and sorting
- Manage product status
- Handle product images
- Support multilingual content

---

# Base URL

```
/api/v1/products
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get products |
| GET | /{id} | Get product details |
| POST | / | Create product |
| PUT | /{id} | Update product |
| PATCH | /{id}/status | Change product status |
| DELETE | /{id} | Delete product |
| POST | /{id}/images | Upload product images |
| DELETE | /{id}/images/{imageId} | Delete product image |
| GET | /search | Search products |
| GET | /featured | Get featured products |
| GET | /latest | Get latest products |

---

# Get Products

### Endpoint

```
GET /api/v1/products
```

### Query Parameters

Supported parameters:

- page
- limit
- category
- seller
- minPrice
- maxPrice
- status
- sort

---

# Get Product Details

### Endpoint

```
GET /api/v1/products/{id}
```

### Response

Returns:

- Product ID
- Name
- Description
- Category
- Seller
- Price
- Discount
- Currency
- Images
- Inventory
- Rating
- Reviews
- Status

---

# Create Product

### Endpoint

```
POST /api/v1/products
```

### Request Body

```json
{
  "name": "Organic Honey",
  "description": "Pure Moroccan Honey",
  "categoryId": 5,
  "price": 180,
  "currency": "MAD",
  "stock": 100
}
```

### Success Response

```json
{
  "success": true,
  "message": "Product created successfully."
}
```

---

# Update Product

### Endpoint

```
PUT /api/v1/products/{id}
```

Allows updating:

- Name
- Description
- Price
- Stock
- Images
- Status
- Category

---

# Product Status

Supported values:

- Draft
- Published
- Hidden
- OutOfStock
- Archived

---

# Product Images

Supported formats:

- JPG
- PNG
- WebP

Recommended features:

- Multiple images
- Image ordering
- Thumbnail generation
- Automatic optimization

---

# Search

### Endpoint

```
GET /api/v1/products/search
```

Supported search options:

- Keyword
- Category
- Seller
- Price Range
- Rating
- Availability

---

# Featured Products

### Endpoint

```
GET /api/v1/products/featured
```

Returns products highlighted by platform administrators.

---

# Latest Products

### Endpoint

```
GET /api/v1/products/latest
```

Returns recently published products.

---

# Validation Rules

Examples include:

- Product name is required
- Price must be greater than zero
- Stock cannot be negative
- Category must exist
- Currency must be supported

---

# Authentication

Public endpoints:

- Product listing
- Product details
- Search

Protected endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

Seller or administrator permissions are required to create, update, or delete products.

---

# Error Responses

Possible errors include:

- UNAUTHORIZED
- FORBIDDEN
- PRODUCT_NOT_FOUND
- INVALID_CATEGORY
- VALIDATION_ERROR
- IMAGE_UPLOAD_FAILED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- File Upload Validation
- Input Sanitization
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- Product variants
- Digital products
- AI-generated descriptions
- 3D product models
- Product comparison
- Barcode and QR code support

---

# Related Documents

- sellers-api.md
- ../09-database/products-table.md
- ../08-features/product-management.md

---

# Conclusion

The Products API provides a comprehensive and scalable foundation for managing products across the Maroc Local Hub marketplace while ensuring consistency, security, and high performance.

---

End of Document
