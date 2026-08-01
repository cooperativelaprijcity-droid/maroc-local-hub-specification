# Categories Table Specification

Document ID: MLH-DB-005
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `categories` table used to organize products within the Maroc Local Hub marketplace.

The category system must support hierarchical product classification, multilingual content, SEO optimization, marketplace navigation, and future category expansion.

---

# Table Name

`categories`

---

# Description

The `categories` table stores the core information for product categories.

The category system should support:

- Main Categories
- Subcategories
- Nested Categories
- Category Hierarchies
- Multilingual Names
- SEO Metadata
- Category Images
- Category Ordering
- Category Visibility

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique category identifier |
| `parent_id` | UUID | No | Foreign Key, Nullable | Parent category identifier |
| `name` | VARCHAR(255) | Yes | Not Null | Default category name |
| `slug` | VARCHAR(255) | Yes | Unique, Not Null | SEO-friendly category identifier |
| `description` | TEXT | No | Nullable | Category description |
| `image_url` | TEXT | No | Nullable | Category image |
| `icon_url` | TEXT | No | Nullable | Category icon |
| `sort_order` | INTEGER | Yes | Default 0 | Category display order |
| `status` | VARCHAR(30) | Yes | Not Null | Category lifecycle status |
| `visibility` | VARCHAR(30) | Yes | Not Null | Category visibility |
| `seo_title` | VARCHAR(255) | No | Nullable | SEO title |
| `seo_description` | TEXT | No | Nullable | SEO description |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Record update timestamp |
| `deleted_at` | TIMESTAMP | No | Nullable | Soft delete timestamp |

---

# Hierarchical Structure

Categories should support parent-child relationships.

Example:

```text
Food & Agriculture
├── Honey
│   ├── Daghmous Honey
│   ├── Sidr Honey
│   └── Multifloral Honey
├── Argan Products
│   ├── Argan Oil
│   └── Argan Cosmetics
└── Cactus Products
    ├── Cactus Fruit
    └── Cactus Seed Oil
```

The `parent_id` field should reference the parent category.

A top-level category should have:

`parent_id = NULL`

A child category should contain the UUID of its parent category.

---

# Category Status

Supported values should include:

- `draft`
- `active`
- `inactive`
- `archived`

---

# Category Visibility

Supported values should include:

- `public`
- `private`
- `hidden`

---

# Field Notes

## `parent_id`

Defines the hierarchical relationship between categories.

Recommended relationship:

`categories.id → categories.parent_id`

The system must prevent circular category relationships.

---

## `name`

Stores the default category name.

Multilingual category names should be managed through a dedicated translation model.

---

## `slug`

The slug should be:

- Unique
- URL-safe
- SEO-friendly
- Stable whenever possible

Example:

```text
food-agriculture
honey
daghmous-honey
```

---

## `sort_order`

Defines the order in which categories appear in navigation and category listings.

Lower values should appear before higher values unless the interface specifies another sorting method.

---

# Multilingual Categories

The platform initially supports:

- Arabic
- French
- English
- Spanish

A dedicated table is recommended:

`category_translations`

Possible fields:

- `id`
- `category_id`
- `language_code`
- `name`
- `description`
- `seo_title`
- `seo_description`
- `created_at`
- `updated_at`

Recommended unique constraint:

`category_id + language_code`

---

# Product Relationships

Products may belong to categories.

For simple implementations, a product may reference one primary category.

For a flexible marketplace architecture, a many-to-many relationship is recommended using:

`product_categories`

This allows one product to appear in multiple relevant categories.

Example:

A Moroccan honey product could belong to:

- Food & Agriculture
- Honey
- Natural Products
- Moroccan Products

---

# Category Navigation

Categories should support navigation through:

- Main Navigation
- Marketplace Navigation
- Product Discovery
- Search Filters
- Seller Product Management

The category hierarchy should remain simple enough for mobile users.

---

# SEO Requirements

Category pages should support:

- SEO-friendly URLs
- Localized URLs
- Meta Titles
- Meta Descriptions
- Canonical URLs
- Hreflang Support

Example:

```text
/en/categories/honey
/fr/categories/miel
/ar/categories/العسل
/es/categories/miel
```

The final URL strategy should be defined by the SEO and architecture specifications.

---

# Indexes

The following indexes are recommended:

- Unique index on `slug`
- Index on `parent_id`
- Index on `status`
- Index on `visibility`
- Index on `sort_order`
- Index on `created_at`
- Index on `deleted_at`

Additional indexes may be required for multilingual search.

---

# Constraints

The table should enforce:

- `name` must not be empty.
- `slug` must be unique.
- `sort_order` must not be negative.
- `parent_id` must reference a valid category when present.
- Categories must not reference themselves as their own parent.
- Circular category relationships must be prevented.
- `status` must contain a valid supported value.
- `visibility` must contain a valid supported value.

---

# Category Lifecycle

A category may follow this lifecycle:

1. Category created.
2. Category remains in `draft`.
3. Administrator reviews category.
4. Category becomes `active`.
5. Category appears in marketplace navigation.
6. Category may become `inactive`.
7. Category may eventually become `archived`.

Existing product relationships should be preserved when a category is archived.

---

# Category Management

Only authorized administrators should be able to:

- Create categories.
- Edit categories.
- Delete or archive categories.
- Change category hierarchy.
- Change category visibility.
- Manage category translations.

Sellers should select from existing categories rather than creating categories directly, unless the platform explicitly enables seller-submitted category suggestions.

---

# Security Considerations

Category management must be protected through role-based access control.

Requirements:

- Only authorized administrators can modify categories.
- Category hierarchy changes must be logged.
- Category deletion must preserve data integrity.
- Category operations should be auditable.

---

# Performance Notes

Categories are frequently accessed by:

- Homepage navigation.
- Marketplace navigation.
- Product pages.
- Search filters.
- Seller dashboards.

The category hierarchy may be cached to improve performance.

---

# Future Enhancements

Future versions may support:

- Category Attributes
- Category-Specific Filters
- Category-Specific Product Fields
- Category Recommendations
- AI-Based Category Classification
- Automatic Product Categorization
- Category Analytics

---

# Related Documents

Related database specifications include:

- `products-table.md`
- `database-relationships.md`
- `indexing-strategy.md`
- `multilingual-support.md`

---

# Conclusion

The `categories` table provides the hierarchical classification system for products across Maroc Local Hub.

The design must support scalable category management, multilingual content, SEO-friendly marketplace navigation, and flexible product-category relationships.

The final implementation should prioritize data integrity, simple navigation, and future marketplace expansion.

---

End of Document
