# Products Table Specification

Document ID: MLH-DB-004
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `products` table used to store marketplace product information.

The product data model must support products offered by sellers, cooperatives, farmers, artisans, manufacturers, local brands, and small businesses.

The model should support product discovery, categories, multilingual content, pricing, product status, inventory, reviews, recommendations, and marketplace operations.

---

# Table Name

`products`

---

# Description

The `products` table stores the core information for every product listed on Maroc Local Hub.

Product-specific information should be separated from related entities such as:

- Product Images
- Product Variants
- Product Categories
- Product Attributes
- Inventory
- Reviews
- Product Translations

This separation improves scalability, maintainability, and data integrity.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique product identifier |
| `seller_id` | UUID | Yes | Foreign Key, Not Null | Seller responsible for the product |
| `category_id` | UUID | No | Foreign Key | Primary product category |
| `brand_id` | UUID | No | Foreign Key | Product brand |
| `name` | VARCHAR(255) | Yes | Not Null | Default product name |
| `slug` | VARCHAR(255) | Yes | Unique, Not Null | SEO-friendly product identifier |
| `short_description` | TEXT | No | Nullable | Short product summary |
| `description` | TEXT | No | Nullable | Full product description |
| `sku` | VARCHAR(100) | No | Unique where applicable | Stock Keeping Unit |
| `product_type` | VARCHAR(50) | Yes | Not Null | Product classification |
| `status` | VARCHAR(30) | Yes | Not Null | Product lifecycle status |
| `visibility` | VARCHAR(30) | Yes | Not Null | Product visibility |
| `base_price` | DECIMAL(12,2) | Yes | Not Null | Base product price |
| `currency_code` | VARCHAR(10) | Yes | Not Null | Currency used for the base price |
| `compare_at_price` | DECIMAL(12,2) | No | Nullable | Previous or reference price |
| `is_featured` | BOOLEAN | Yes | Default false | Whether product is featured |
| `published_at` | TIMESTAMP | No | Nullable | Publication timestamp |
| `created_at` | TIMESTAMP | Yes | Not Null | Record creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Record update timestamp |
| `deleted_at` | TIMESTAMP | No | Nullable | Soft delete timestamp |

---

# Product Types

The platform may support product types such as:

- `physical`
- `digital`
- `service`
- `bundle`

The initial marketplace implementation should prioritize physical products.

Future product types should be introduced only when supported by the relevant checkout, payment, fulfillment, and delivery workflows.

---

# Product Status

Supported values should include:

- `draft`
- `pending_review`
- `approved`
- `rejected`
- `published`
- `archived`
- `suspended`

---

# Product Visibility

Supported values should include:

- `public`
- `private`
- `unlisted`

---

# Field Notes

## `seller_id`

References the seller responsible for the product.

Recommended relationship:

` sellers.id → products.seller_id `

A product should belong to one primary seller.

---

## `category_id`

References the primary category of the product.

The platform may later support multiple categories through a dedicated junction table such as:

`product_categories`

---

## `brand_id`

References the product brand when applicable.

Products without a formal brand may leave this field null.

---

## `name`

Stores the default product name.

Multilingual product names should be handled through a dedicated translation model when required.

---

## `slug`

The slug should be unique and optimized for SEO.

Example:

`natural-moroccan-daghmous-honey`

---

## `sku`

The SKU identifies a sellable product or inventory unit.

If products have multiple variants, SKU management should preferably be handled at the variant level.

---

## `base_price`

Stores the default product price.

Financial calculations should use appropriate decimal precision.

Floating-point types should not be used for financial amounts.

---

## `currency_code`

Stores the currency associated with the base price.

Examples:

- `MAD`
- `EUR`
- `USD`

The platform should preserve the currency used for historical orders.

---

# Product Variants

Products may have multiple variants.

Examples include:

- Size
- Color
- Weight
- Packaging
- Quantity

Examples:

- Honey 250g
- Honey 500g
- Honey 1kg

Variant data should be stored in a separate table:

`product_variants`

Each variant may contain:

- Variant ID
- Product ID
- SKU
- Price
- Currency
- Weight
- Dimensions
- Stock Reference
- Status

The exact schema should be defined in a future dedicated document if variants become a core requirement.

---

# Product Images

Product images should not be stored directly as binary data inside the `products` table.

Images should be stored using:

- Object Storage
- CDN
- Secure Media Storage

A dedicated table such as:

`product_images`

should store:

- Image ID
- Product ID
- Image URL
- Alt Text
- Sort Order
- Is Primary
- Created At

---

# Multilingual Products

The platform supports:

- Arabic
- French
- English
- Spanish

For multilingual product content, a dedicated table is recommended:

`product_translations`

Possible fields:

- `id`
- `product_id`
- `language_code`
- `name`
- `short_description`
- `description`
- `created_at`
- `updated_at`

A unique constraint should prevent duplicate translations for the same product and language.

Recommended unique combination:

`product_id + language_code`

---

# Product Attributes

Product-specific attributes should be modeled separately when possible.

Examples:

- Material
- Origin
- Weight
- Ingredients
- Certification
- Production Method

A flexible attribute model may include:

- `product_attributes`
- `attribute_definitions`
- `attribute_values`

The final implementation should balance flexibility with query performance.

---

# Categories

Products should be associated with categories.

The platform should support:

- Main Categories
- Subcategories
- Category Hierarchies

If multiple category assignments are required, use:

`product_categories`

instead of relying exclusively on `category_id`.

---

# Inventory

Inventory should not be stored directly in the `products` table.

Inventory should be managed through dedicated inventory entities.

Examples:

- `inventory`
- `inventory_movements`
- `warehouses`

The product record should remain focused on product identity and catalog information.

---

# Reviews and Ratings

Products may receive customer reviews and ratings.

Review data should be stored in a dedicated table:

`reviews`

The product record may optionally maintain cached values such as:

- `rating_average`
- `rating_count`

These values should be updated safely and consistently.

---

# Recommendations

Products may be used by the recommendation engine.

Recommendation data should not be stored directly in the product record.

The recommendation system may reference:

- Product ID
- User ID
- Recommendation Type
- Recommendation Score
- Recommendation Source

---

# Indexes

The following indexes are recommended:

- Unique index on `slug`
- Index on `seller_id`
- Index on `category_id`
- Index on `brand_id`
- Index on `status`
- Index on `visibility`
- Index on `product_type`
- Index on `published_at`
- Index on `created_at`
- Index on `deleted_at`

Additional search indexes may be introduced for:

- Product Name
- Description
- SKU
- Category
- Seller

---

# Constraints

The table should enforce:

- `seller_id` must reference a valid seller.
- `name` must not be empty.
- `slug` must be unique.
- `base_price` must not be negative.
- `compare_at_price` must not be negative.
- `currency_code` must be valid.
- `status` must contain a supported value.
- `visibility` must contain a supported value.

---

# Product Publishing Workflow

A typical product lifecycle is:

1. Seller creates a product.
2. Product enters `draft` status.
3. Seller submits the product.
4. Product enters `pending_review`.
5. Administrator reviews the product.
6. Product is `approved` or `rejected`.
7. Approved product becomes `published`.
8. Product may later become `archived` or `suspended`.

The exact workflow should align with the platform's seller and moderation features.

---

# Security Considerations

Product data must be protected from unauthorized modification.

Requirements:

- Only authorized sellers can modify their products.
- Administrators can moderate products.
- Seller ownership must be validated.
- Product moderation actions should be auditable.
- Deleted products should be soft deleted when historical data must be preserved.

---

# Performance Notes

The `products` table will be heavily queried.

Common operations include:

- Product listing
- Product search
- Category browsing
- Seller storefronts
- Product recommendations
- Product detail pages

Performance strategies may include:

- Proper indexing
- Pagination
- Caching
- Search engine integration
- CDN-based image delivery

---

# Data Lifecycle

Product records should support:

1. Product creation.
2. Draft editing.
3. Submission for review.
4. Approval or rejection.
5. Publication.
6. Updates.
7. Suspension or archiving.
8. Soft deletion.

Historical order records must continue to reference products even if those products are no longer publicly available.

---

# Future Enhancements

Future versions
