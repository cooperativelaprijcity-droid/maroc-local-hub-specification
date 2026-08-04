# Users API

Document ID: MLH-API-003

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Users API for Maroc Local Hub.

It specifies all endpoints related to user profile management, account settings, preferences, addresses, and account lifecycle operations.

---

# Objectives

The Users API should:

- Manage user profiles
- Update personal information
- Manage addresses
- Store user preferences
- Support account deletion
- Provide secure profile access

---

# Base URL

```
/api/v1/users
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /me | Get current user profile |
| PUT | /me | Update current user profile |
| PATCH | /me/avatar | Update profile picture |
| PUT | /me/password | Change password |
| GET | /me/addresses | Get user addresses |
| POST | /me/addresses | Create address |
| PUT | /me/addresses/{id} | Update address |
| DELETE | /me/addresses/{id} | Delete address |
| GET | /me/preferences | Get preferences |
| PUT | /me/preferences | Update preferences |
| DELETE | /me | Delete account |

---

# Get Profile

### Endpoint

```
GET /api/v1/users/me
```

### Authentication

Bearer Token Required.

### Success Response

```json
{
  "id": 1,
  "firstName": "Ahmed",
  "lastName": "Ali",
  "email": "user@example.com",
  "phone": "+212600000000",
  "role": "customer"
}
```

---

# Update Profile

### Endpoint

```
PUT /api/v1/users/me
```

### Request Body

```json
{
  "firstName": "Ahmed",
  "lastName": "Ali",
  "phone": "+212600000000"
}
```

---

# Update Avatar

### Endpoint

```
PATCH /api/v1/users/me/avatar
```

Supports uploading a profile image.

Accepted formats:

- JPG
- PNG
- WebP

---

# Change Password

### Endpoint

```
PUT /api/v1/users/me/password
```

### Request Body

```json
{
  "currentPassword": "********",
  "newPassword": "********"
}
```

---

# Address Management

Each user may store multiple delivery addresses.

Address fields include:

- Full Name
- Phone Number
- Country
- Region
- City
- Postal Code
- Street Address
- Default Address

---

# User Preferences

Preferences may include:

- Preferred Language
- Preferred Currency
- Notification Settings
- Dark Mode
- Marketing Preferences

---

# Delete Account

### Endpoint

```
DELETE /api/v1/users/me
```

Account deletion should require confirmation and follow the platform's data retention policy.

---

# Validation Rules

Examples include:

- Email format validation
- Phone number validation
- Password complexity requirements
- Required fields validation

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Error Responses

Possible errors include:

- UNAUTHORIZED
- VALIDATION_ERROR
- RESOURCE_NOT_FOUND
- INVALID
