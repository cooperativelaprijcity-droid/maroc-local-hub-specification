# Button System

> Document ID: MLH-UI-001
>
> Version: 1.0.0
>
> Status: Draft
>
> Owner: Maroc Local Hub

---

## Purpose

This document defines the button system for Maroc Local Hub.

Buttons must provide clear, consistent, accessible, and responsive interactions across the website and mobile applications.

---

## Design Principles

Buttons must follow these principles:

- Clear and easy to understand
- Consistent across the platform
- Accessible for all users
- Touch-friendly on mobile devices
- Responsive on all screen sizes
- Compatible with Arabic RTL layouts
- Visually aligned with the Maroc Local Hub brand

---

## Button Types

### Primary Button

Used for the main action on a page.

Examples:

- Buy Now
- Add to Cart
- Start Shopping
- Register
- Become a Seller

---

### Secondary Button

Used for supporting actions.

Examples:

- Learn More
- View Details
- Explore Products
- Contact Seller

---

### Outline Button

Used for secondary actions when a lighter visual emphasis is required.

Examples:

- View All
- Cancel
- Back

---

### Text Button

Used for low-priority actions.

Examples:

- Skip
- Read More
- View More

---

### Danger Button

Used for destructive or irreversible actions.

Examples:

- Delete
- Remove Product
- Cancel Order

---

## Button States

Every interactive button must support the following states:

- Default
- Hover
- Focus
- Active
- Disabled
- Loading

---

## Button Sizes

### Small

Used for compact interfaces and secondary actions.

### Medium

Default button size for most platform interactions.

### Large

Used for important calls to action and mobile interfaces.

---

## Accessibility

All buttons must:

- Have a clear accessible label
- Provide visible focus states
- Maintain sufficient color contrast
- Be keyboard accessible
- Support screen readers
- Not rely only on color to communicate meaning

---

## Mobile Requirements

Buttons must be easy to tap on mobile devices.

Primary actions should have a minimum touch target of approximately 44 × 44 px.

Buttons should adapt to different screen sizes without breaking the layout.

---

## RTL Support

The button system must support Arabic RTL interfaces.

Icons and directional elements must automatically adapt to the writing direction where appropriate.

---

## Icon Usage

Icons may be used with buttons when they improve clarity.

Icons must:

- Have consistent visual style
- Be properly aligned with text
- Support RTL layouts
- Not replace important text unless the meaning is universally understood

---

## Localization

Button labels must support:

- Arabic
- English
- French
- Spanish

Text length may change depending on the language, so buttons must be flexible and responsive.

---

## Recommended Examples

Primary:

`Add to Cart`

Secondary:

`View Details`

Outline:

`Explore Products`

Danger:

`Delete`

Loading:

`Processing...`

---

## Usage Rules

Do:

- Use one primary action per section when possible
- Use clear action-oriented labels
- Maintain consistent spacing
- Follow the official brand and design system

Do not:

- Use too many primary buttons together
- Use unclear labels
- Create inconsistent button styles
- Use buttons for navigation when a link is more appropriate

---

## Related Documents

- `04-design-system/color-system.md`
- `04-design-system/spacing.md`
- `04-design-system/typography.md`
- `04-design-system/icons.md`
- `03-brand/brand-guidelines.md`

---

## Status

This document is part of the Maroc Local Hub Engineering & Design Specification.

Future versions may define exact:

- Colors
- Typography
- Border radius
- Shadows
- Padding
- Icon sizes
- Animation
- Component API
