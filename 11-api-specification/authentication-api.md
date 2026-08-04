# Authentication API

Document ID: MLH-API-002

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines the Authentication API for Maroc Local Hub.

It specifies the endpoints responsible for user authentication, account registration, session management, token lifecycle, password recovery, and email verification.

---

# Objectives

The Authentication API should:

- Authenticate users securely
- Protect user accounts
- Support JWT authentication
- Support refresh tokens
- Support password recovery
- Support email verification
- Support logout from active sessions

---

# Base URL

```
/api/v1/auth
```

---

# Authentication Endpoints

| Method | Endpoint | Description |
|----------|---------------------------|----------------------------|
| POST | /register | Create a new account |
| POST | /login | User login |
| POST | /refresh-token | Refresh access token |
| POST | /logout | Logout current session |
| POST | /forgot-password | Request password reset |
| POST | /reset-password | Reset password |
| POST | /verify-email | Verify email address |
| POST | /resend-verification | Resend verification email |
| GET | /me | Get authenticated user |

---

# Register

### Endpoint

```
POST /api/v1/auth/register
```

### Request Body

```json
{
  "firstName": "Ahmed",
  "lastName": "Ali",
  "email": "user@example.com",
  "password": "********",
  "phone": "+212600000000"
}
```

### Success Response

```json
{
  "success": true,
  "message": "Account created successfully."
}
```

---

# Login

### Endpoint

```
POST /api/v1/auth/login
```

### Request Body

```json
{
  "email": "user@example.com",
  "password": "********"
}
```

### Success Response

```json
{
  "accessToken": "...",
  "refreshToken": "...",
  "expiresIn": 3600
}
```

---

# Refresh Token

### Endpoint

```
POST /api/v1/auth/refresh-token
```

### Purpose

Generate a new access token using a valid refresh token.

---

# Logout

### Endpoint

```
POST /api/v1/auth/logout
```

### Purpose

Invalidate the current authenticated session.

---

# Forgot Password

### Endpoint

```
POST /api/v1/auth/forgot-password
```

### Purpose

Send a password reset link or verification code.

---

# Reset Password

### Endpoint

```
POST /api/v1/auth/reset-password
```

### Purpose

Allow users to create a new password after successful verification.

---

# Verify Email

### Endpoint

```
POST /api/v1/auth/verify-email
```

### Purpose

Verify ownership of the user's email address.

---

# Resend Verification

### Endpoint

```
POST /api/v1/auth/resend-verification
```

### Purpose

Send a new verification email if the previous one expired.

---

# Get Current User

### Endpoint

```
GET /api/v1/auth/me
```

### Authentication

Bearer Token Required.

### Success Response

```json
{
  "id": 1,
  "name": "Ahmed Ali",
  "email": "user@example.com",
  "role": "customer"
}
```

---

# Validation Rules

Examples include:

- Email must be unique
- Password must meet security requirements
- Phone number format must be valid
- Required fields cannot be empty

---

# Authentication

Protected endpoints require:

```
Authorization: Bearer <JWT_TOKEN>
```

---

# Error Responses

Possible errors include:

- INVALID_CREDENTIALS
- EMAIL_ALREADY_EXISTS
- EMAIL_NOT_VERIFIED
- TOKEN_EXPIRED
- INVALID_TOKEN
- ACCOUNT_DISABLED
- VALIDATION_ERROR

---

# Security Requirements

The API should support:

- HTTPS only
- Password hashing
- JWT expiration
- Refresh token rotation
- Rate limiting
- Brute-force protection
- Audit logging

---

# Future Enhancements

Future authentication features may include:

- Multi-Factor Authentication (MFA)
- Passkeys
- Social Login
- Biometric Authentication

---

# Related Documents

- README.md
- ../10-architecture/authentication-architecture.md
- ../10-architecture/security-architecture.md

---

# Conclusion

The Authentication API provides a secure and scalable identity management foundation for Maroc Local Hub while following modern authentication and authorization best practices.

---

End of Document
