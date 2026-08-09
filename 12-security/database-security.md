# Database Security

Document ID: MLH-SEC-008

Version: 1.0.0

Status: Draft

Owner: Maroc Local Hub

---

# Purpose

This document defines database security requirements for the Maroc Local Hub platform.

The objective is to protect databases and stored data against unauthorized access, modification, disclosure, corruption, and loss.

---

# Database Security Objectives

The database security architecture should:

- Protect sensitive data
- Restrict database access
- Enforce least privilege
- Protect database credentials
- Prevent unauthorized data modification
- Protect backups
- Maintain database auditability
- Support secure recovery
- Reduce the database attack surface

---

# Database Architecture

Database access should follow controlled application paths.

```text
Client
  |
  v
Frontend / Mobile
  |
  v
API
  |
  v
Application Services
  |
  v
Database
```

Clients should not receive unrestricted direct access to the database.

---

# Database Access Control

Database access must follow the principle of least privilege.

Access should be granted according to:

- Service responsibility
- User role
- Environment
- Required operations
- Data sensitivity

---

# Database Accounts

Separate database identities should be used for different services where appropriate.

Examples:

```text
Application Service
Reporting Service
Migration Service
Administrative Service
Backup Service
```

Each identity should receive only the permissions required for its function.

---

# Production Database

Production database access must be strongly restricted.

Production access should:

- Require authentication
- Be limited to authorized personnel and services
- Be monitored
- Be audited where appropriate
- Avoid unnecessary direct access

Development tools and AI agents should not receive unrestricted production database access.

---

# Development Database

Development environments should use separate databases from production.

Where possible:

```text
Development
     |
     v
Staging
     |
     v
Production
```

Development data should not automatically contain real sensitive production information.

---

# Database Credentials

Database credentials must:

- Never be committed to source control
- Never be hard-coded in application code
- Never be exposed in frontend applications
- Not be stored in plain text where avoidable
- Be rotated when necessary
- Be protected using appropriate secrets-management mechanisms

---

# Database Encryption

Sensitive database data should be protected using appropriate encryption controls.

Encryption may include:

- Storage encryption
- Database encryption
- Field-level encryption where required
- Encrypted backups

Encryption requirements should be based on data classification and risk.

---

# SQL Injection Protection

Applications must protect databases against SQL injection.

Controls include:

- Parameterized queries
- Prepared statements
- Safe ORM usage
- Input validation
- Avoiding unsafe dynamic SQL

User input must never be concatenated directly into SQL queries.

---

# NoSQL Injection

If NoSQL databases are used, application queries must also be protected against malicious query manipulation.

The same principle of validating and safely handling untrusted input applies.

---

# Database Schema Security

Database schemas should be designed with security in mind.

Security considerations include:

- Appropriate constraints
- Foreign keys
- Unique constraints
- Data validation
- Controlled relationships
- Avoiding unnecessary sensitive fields

---

# Data Integrity

Database integrity should be protected using:

- Primary keys
- Foreign keys
- Unique constraints
- Check constraints
- Transactions
- Referential integrity

Application-level validation should complement database constraints.

---

# Transactions

Critical operations should use database transactions where required.

Examples include:

- Order creation
- Inventory updates
- Payment state changes
- Refund processing
- Commission calculations

Transactions should help maintain consistent database state.

---

# Row-Level Security

Where supported and appropriate, Row-Level Security (RLS) may be used to restrict access to individual records.

Example:

```text
Customer A
    |
    +---- Own Orders → Allowed

Customer A
    |
    +---- Customer B Orders → Denied
```

RLS policies must be carefully designed and tested.

---

# Sensitive Data

Sensitive database information may include:

- Personal information
- Authentication-related data
- Seller information
- Financial information
- Transaction records
- Security logs

Access to sensitive data must be restricted.

---

# Password Data

Passwords must never be stored in plaintext.

Password storage must use an appropriate password-hashing mechanism.

Database administrators should not be able to retrieve users' original passwords.

---

# Database Backups

Database backups should be:

- Created regularly
- Protected from unauthorized access
- Encrypted where appropriate
- Monitored
- Tested for restoration
- Retained according to policy

---

# Backup Isolation

Backups should be protected from compromise of the primary database.

Where appropriate, backups should use:

- Separate credentials
- Separate storage
- Access restrictions
- Independent security controls

---

# Database Monitoring

Database activity should be monitored for suspicious behavior.

Examples include:

- Unexpected access
- Repeated authentication failures
- Unusual queries
- Large data exports
- Unauthorized schema changes
- Privilege changes

---

# Database Logging

Security-relevant database events should be logged where appropriate.

Examples:

- Authentication events
- Permission changes
- Schema changes
- Administrative actions
- Sensitive data access

Logs must not unnecessarily expose sensitive values.

---

# Database Auditing

Critical database operations should be auditable.

Audit information may include:

```text
Actor
Action
Resource
Timestamp
Result
Environment
```

Audit logs should be protected against unauthorized modification.

---

# Schema Changes

Database migrations must be controlled.

Migration processes should:

- Be version controlled
- Be reviewed
- Be tested
- Be executed against the correct environment
- Have rollback or recovery procedures where practical

Production migrations require additional controls.

---

# Production Migration

Production database migrations should be performed through controlled deployment processes.

High-risk migrations should require appropriate review and approval.

Examples:

- Dropping columns
- Changing critical constraints
- Large data transformations
- Changing financial tables

---

# AI and Database Access

AI agents must not receive unrestricted database access.

Recommended model:

```text
AI Agent
   |
   v
MCP / Approved Tool
   |
   v
