# Categories API

Document ID: MLH-API-006

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Categories API for Maroc Local Hub.

It specifies all endpoints related to category management, hierarchy, navigation, localization, and category administration.

---

# Objectives

The Categories API should:

- Organize products into logical categories
- Support hierarchical categories
- Enable multilingual category names
- Improve navigation and search
- Support category management by administrators

---

# Base URL

```
/api/v1/categories
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get all categories |
| GET | /tree | Get category hierarchy |
| GET | /{id} | Get category details |
| POST | / | Create category |
| PUT | /{id} | Update category |
| PATCH | /{id}/status | Update category status |
| DELETE | /{id} | Delete category |
| GET | /{id}/products | Get products by category |

---

# Get Categories

### Endpoint

```
GET /api/v1/categories
```

Returns all active categories.

---

# Category Tree

### Endpoint

```
GET /api/v1/categories/tree
```

Returns the complete hierarchical structure.

Example:

```text
Food
 ├── Honey
 ├── Olive Oil
 └── Dates

Crafts
 ├── Pottery
 ├── Carpets
 └── Leather
```

---

# Get Category Details

### Endpoint

```
GET /api/v1/categories/{id}
```

Returns:

- Category ID
- Name
- Slug
- Parent Category
- Description
- Icon
- Image
- Status

---

# Create Category

### Endpoint

```
POST /api/v1/categories
```

### Request Body

```json
{
  "name": "Honey",
  "slug": "honey",
  "parentId": null,
  "description": "Natural honey products"
}
```

---

# Update Category

### Endpoint

```
PUT /api/v1/categories/{id}
```

Allows updating:

- Name
- Description
- Parent Category
- Icon
- Cover Image
- Status

---

# Category Status

Supported values:

- Active
- Hidden
- Archived

---

# Get Products by Category

### Endpoint

```
GET /api/v1/categories/{id}/products
```

Supports:

- Pagination
- Sorting
- Filtering
- Price Range
- Rating
- Availability

---

# Validation Rules

Examples include:

- Category name is required
- Slug must be unique
- Parent category must exist
- Circular references are not allowed

---

# Authentication

Public endpoints:

- Category listing
- Category tree
- Category details

Protected endpoints:

- Create
- Update
- Delete

Require:

```
Authorization: Bearer <JWT_TOKEN>
```

Administrator permissions are required for category management.

---

# Error Responses

Possible errors include:

- CATEGORY_NOT_FOUND
- CATEGORY_ALREADY_EXISTS
- INVALID_PARENT_CATEGORY
- VALIDATION_ERROR
- UNAUTHORIZED
- FORBIDDEN

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- Input Validation
- Audit Logging

---

# Future Enhancements

Future improvements may include:

- AI-generated category suggestions
- Dynamic category recommendations
- Category analytics
- Seasonal categories
- Smart navigation

---

# Related Documents

- products-api.md
- ../09-database/categories-table.md
- ../08-features/category-management.md

---

# Conclusion

The Categories API provides a scalable and organized classification system for products, enabling efficient browsing, filtering, and administration across the Maroc Local Hub marketplace.

---

End of Document
