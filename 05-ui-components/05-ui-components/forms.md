# Form Components System

Document ID: MLH-UI-002  
Version: 1.0.0  
Status: Draft  
Owner: Maroc Local Hub

## Purpose

This document defines the form components used across Maroc Local Hub.

Forms must be simple, accessible, responsive, and optimized for mobile-first experiences.

## Form Components

The platform should support:

- Text Input
- Email Input
- Password Input
- Phone Input
- Search Input
- Textarea
- Select
- Checkbox
- Radio Button
- Toggle Switch
- Date Picker
- File Upload
- Image Upload

## Input States

Every form field must support:

- Default
- Hover
- Focus
- Filled
- Disabled
- Read-only
- Loading
- Success
- Error

## Labels

Every form field should have a clear and descriptive label.

Labels should remain visible whenever possible.

Avoid relying only on placeholder text as a label.

## Placeholder

Placeholders may provide additional guidance.

Example:

Enter your product name

Placeholders must not replace field labels.

## Required Fields

Required fields must be clearly identified.

Recommended indicator:

*

Example:

Product Name *

## Validation

Form validation should provide immediate and understandable feedback.

### Success

Use a positive visual indicator.

Example:

Product successfully saved.

### Error

Error messages must explain:

- What went wrong
- Where the problem is
- How to fix it

Example:

Please enter a valid email address.

## Error Handling

Errors should appear close to the relevant field.

Do not use technical error messages that users cannot understand.

## Form Layout

Forms should use:

- Clear vertical spacing
- Logical field grouping
- Consistent label placement
- Clear section headings

## Mobile Experience

Forms must be optimized for smartphones.

Requirements:

- Minimum touch target: 44px
- Large and readable text
- Comfortable spacing
- Mobile-friendly keyboard types
- Avoid unnecessary fields

## Accessibility

Forms must:

- Support keyboard navigation
- Support screen readers
- Have visible focus states
- Provide accessible labels
- Maintain sufficient color contrast
- Clearly communicate validation errors

## RTL Support

Forms must support Arabic RTL layouts.

The following elements should adapt automatically:

- Text alignment
- Labels
- Input direction
- Icons
- Validation messages
- Navigation controls

## Localization

The form system must support:

- Arabic
- French
- English
- Spanish

Form layouts must accommodate different text lengths.

## E-commerce Forms

The platform should support specialized forms for:

- Customer Registration
- Seller Registration
- Product Creation
- Checkout
- Shipping Address
- Payment
- Contact Seller
- Product Reviews

## Design Principles

Forms must be:

- Simple
- Clear
- Fast
- Accessible
- Trustworthy
- Mobile-first

The goal is to reduce user friction and maximize successful form completion.
