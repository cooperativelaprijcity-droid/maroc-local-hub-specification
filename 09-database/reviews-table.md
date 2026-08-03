# Reviews Table Specification

Document ID: MLH-DB-012
Version: 1.0.0
Status: Draft
Owner: Maroc Local Hub

---

# Purpose

This document defines the `reviews` table used to store customer reviews and ratings for products and sellers on Maroc Local Hub.

The review system should help customers share their experiences while providing reliable feedback about product quality and seller performance.

---

# Table Name

`reviews`

---

# Description

The `reviews` table stores customer-generated ratings and written reviews.

A review may be associated with:

- A Product
- A Seller
- An Order
- An Order Item
- A Customer

Reviews should preferably be linked to verified purchases to improve trust and reduce fraudulent reviews.

---

# Primary Key

- `id` (UUID)

The primary key must be globally unique and non-sequential.

---

# Recommended Columns

| Column | Type | Required | Constraints | Description |
|---|---|---:|---|---|
| `id` | UUID | Yes | Primary Key, Not Null | Unique review identifier |
| `user_id` | UUID | Yes | Foreign Key, Not Null | Customer who submitted the review |
| `product_id` | UUID | No | Foreign Key, Nullable | Reviewed product |
| `seller_id` | UUID | No | Foreign Key, Nullable | Reviewed seller |
| `order_id` | UUID | No | Foreign Key, Nullable | Related customer order |
| `order_item_id` | UUID | No | Foreign Key, Nullable | Related purchased item |
| `rating` | INTEGER | Yes | 1–5 | Numerical rating |
| `title` | VARCHAR(255) | No | Nullable | Review title |
| `content` | TEXT | No | Nullable | Written review |
| `is_verified_purchase` | BOOLEAN | Yes | Default false | Indicates verified purchase |
| `status` | VARCHAR(30) | Yes | Not Null | Review moderation status |
| `moderation_reason` | TEXT | No | Nullable | Reason for moderation decision |
| `created_at` | TIMESTAMP | Yes | Not Null | Review creation timestamp |
| `updated_at` | TIMESTAMP | Yes | Not Null | Last update timestamp |
| `published_at` | TIMESTAMP | No | Nullable | Publication timestamp |
| `deleted_at` | TIMESTAMP | No | Nullable | Soft delete timestamp |

---

# Rating

The `rating` field represents the customer's numerical evaluation.

Allowed values:

```text
1
2
3
4
5
```

Where:

```text
1 = Very Poor
2 = Poor
3 = Average
4 = Good
5 = Excellent
```

The database must prevent values outside the supported range.

---

# Review Types

The platform may support different review targets.

Possible review types include:

- `product`
- `seller`

A review should normally target at least one of:

- `product_id`
- `seller_id`

The exact business rule should be enforced at the application and database levels where possible.

---

# Product Reviews

Product reviews evaluate the customer's experience with a specific product.

Examples include:

- Product Quality
- Product Description Accuracy
- Packaging
- Value for Money
- Product Satisfaction

The initial implementation may use one overall rating.

Future versions may support multiple rating dimensions.

---

# Seller Reviews

Seller reviews evaluate the seller's service.

Examples include:

- Communication
- Shipping Speed
- Packaging
- Customer Service
- Order Accuracy

Seller ratings may be calculated separately from product ratings.

---

# Verified Purchases

The platform should prioritize reviews from verified customers.

A verified purchase means that:

- The customer placed an order.
- The order included the reviewed product or seller.
- The order was successfully completed or delivered.

The `is_verified_purchase` field indicates whether the review has been verified.

This value should be determined by the platform rather than manually selected by the customer.

---

# One Review Per Purchase

The platform may restrict customers from submitting duplicate reviews for the same purchase.

A possible rule is:

```text
One customer
+
One order item
=
One product review
```

The exact uniqueness constraint should be determined based on the final review and return policy.

---

# Review Status

Supported values should include:

- `pending`
- `published`
- `rejected`
- `hidden`
- `flagged`
- `deleted`

---

# Review Moderation

Reviews may require moderation before publication.

The moderation process may include:

1. Customer submits review.
2. Review enters `pending`.
3. Automated moderation checks content.
4. Human moderation may occur when required.
5. Review becomes `published` or `rejected`.

Reviews may later be:

- Hidden
- Flagged
- Deleted

Moderation actions should be auditable.

---

# Review Content

Review content should be subject to platform rules.

The platform may restrict:

- Spam
- Abusive Language
- Hate Speech
- Personal Information
- Fraudulent Content
- Advertising
- Malicious Links

The final content moderation policy should be documented separately.

---

# Review Images

Future versions may allow customers to attach images to reviews.

Images should not be stored directly in the `reviews` table.

A dedicated table is recommended:

`review_images`

Possible fields:

- `id`
- `review_id`
- `image_url`
- `alt_text`
- `sort_order`
- `created_at`

---

# Review Replies

Sellers may be allowed to respond to customer reviews.

A dedicated table is recommended:

`review_replies`

Possible fields:

- `id`
- `review_id`
- `seller_id`
- `content`
- `status`
- `created_at`
- `updated_at`

Seller replies should not modify the original customer review.

---

# Review Helpful Votes

Customers may indicate whether a review was helpful.

A dedicated table may be introduced:

`review_helpful_votes`

Possible fields:

- `
