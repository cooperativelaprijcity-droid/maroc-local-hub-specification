# Notifications API

Document ID: MLH-API-014

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Notifications API for Maroc Local Hub.

It specifies all endpoints related to user notifications, delivery channels, notification preferences, read status, and system alerts.

---

# Objectives

The Notifications API should:

- Deliver real-time notifications
- Support multiple notification channels
- Manage notification preferences
- Track notification status
- Improve user engagement
- Ensure reliable message delivery

---

# Base URL

```
/api/v1/notifications
```

---

# Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | / | Get user notifications |
| GET | /{id} | Get notification details |
| PATCH | /{id}/read | Mark notification as read |
| PATCH | /read-all | Mark all notifications as read |
| DELETE | /{id} | Delete notification |
| GET | /preferences | Get notification preferences |
| PUT | /preferences | Update notification preferences |
| POST | /test | Send test notification (Admin) |

---

# Get Notifications

### Endpoint

```
GET /api/v1/notifications
```

Supports:

- Pagination
- Filtering
- Read/Unread Status
- Notification Type

---

# Notification Details

### Endpoint

```
GET /api/v1/notifications/{id}
```

Returns:

- Notification ID
- Title
- Message
- Type
- Status
- Created Date
- Read Date

---

# Mark as Read

### Endpoint

```
PATCH /api/v1/notifications/{id}/read
```

Updates the notification status to **Read**.

---

# Mark All as Read

### Endpoint

```
PATCH /api/v1/notifications/read-all
```

Marks every unread notification for the authenticated user as read.

---

# Delete Notification

### Endpoint

```
DELETE /api/v1/notifications/{id}
```

Removes the selected notification.

---

# Notification Preferences

### Endpoint

```
GET /api/v1/notifications/preferences
```

Returns current user preferences.

### Update Preferences

```
PUT /api/v1/notifications/preferences
```

Example:

```json
{
  "email": true,
  "sms": false,
  "push": true,
  "marketing": false
}
```

---

# Notification Types

Supported notification types:

- Order Updates
- Payment Confirmation
- Shipping Updates
- Promotions
- Account Security
- System Announcements

---

# Delivery Channels

Supported channels:

- In-App
- Email
- SMS
- Push Notifications

Future support:

- WhatsApp
- Telegram

---

# Validation Rules

Examples include:

- Notification must exist
- User must own the notification
- Valid preference values required

---

# Authentication

All endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Error Responses

Possible errors include:

- NOTIFICATION_NOT_FOUND
- UNAUTHORIZED
- VALIDATION_ERROR
- ACCESS_DENIED

---

# Security Requirements

The API should support:

- HTTPS
- JWT Authentication
- Role-Based Authorization
- Audit Logging
- Rate Limiting

---

# Future Enhancements

Future improvements may include:

- AI-prioritized notifications
- Smart notification scheduling
- Rich media notifications
- Multilingual notification templates

---

# Related Documents

- users-api.md
- orders-api.md
- authentication-api.md

---

# Conclusion

The Notifications API provides a flexible, secure, and scalable notification system that keeps users informed while respecting their communication preferences.

---

End of Document
