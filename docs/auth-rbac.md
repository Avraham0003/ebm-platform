# Authentication & RBAC – EBM Platform

## Overview
EBM uses **JWT-based authentication** and **Role-Based Access Control (RBAC)** to secure access across multiple clients (User App, Manager App, Admin flows).

The goal is to ensure:
- Only authenticated users can access the platform
- Every request is authorized based on **role** + **resource ownership**
- Permissions are enforced centrally on the backend (server-side)

---

## Roles
EBM supports multiple roles to represent real-world responsibilities:

- **Boss** – Company owner / full control over company resources
- **Manager** – Manages buildings, tasks, professionals, and daily operations
- **Professional (Pro)** – Executes assigned tasks and updates statuses
- **Tenant (User)** – Reports issues, views updates, and receives notifications

> Note: Exact role names may differ between repos. This document describes the conceptual RBAC model.

---

## Authentication Flow (JWT)

### 1) Login / Verification
- User authenticates (e.g., OTP / phone verification, or another configured method)
- Backend validates identity and role
- Backend issues:
  - **Access Token (JWT)** — short/medium-lived token used for API access
  - (Optional) refresh strategy if implemented

### 2) Client Requests
Clients send requests with:
- `Authorization: Bearer <JWT>`

### 3) Server Validation (Middleware)
Every protected endpoint runs:
- Token verification (signature + expiry)
- Extraction of claims (userId, role, companyId, etc.)
- Attach a `req.user` context for authorization checks

---

## JWT Claims (Typical)
The JWT usually includes:
- `sub` / `userId` – the authenticated identity
- `role` – Boss / Manager / Pro / Tenant
- `companyId` – active tenant scope (multi-tenant boundary)
- (Optional) `buildingIds` or other scope hints (server remains source of truth)

> Important: The server must not rely solely on client-provided scope.  
> It should always validate resource ownership and permissions in the database.

---

## Authorization Model (RBAC + Ownership)

EBM authorization is built on two layers:

### 1) Role Permissions (RBAC)
Defines which actions each role can perform:
- Example: Tenant can create a request; Manager can assign a Pro; Boss can manage company-level settings.

### 2) Resource Ownership / Scope
Even if a role is allowed to do an action, it must be within allowed scope:
- Same `companyId`
- Assigned building
- Owner/assignee of a task
- Tenant belongs to the building for which the request is created

---

## Permission Examples

### Company & Buildings
- **Boss**
  - Create/update company settings
  - Add/remove buildings
  - Manage managers/pros
- **Manager**
  - Manage buildings assigned to the company
  - Create/assign tasks
  - View company operational data
- **Pro**
  - View tasks assigned to them
  - Update task status / upload completion photo
- **Tenant**
  - View building notices/documents (as permitted)
  - Report issues / open requests

### Tasks (Missions)
- **Tenant**
  - Create a task/request for their building (if enabled)
  - View own requests and statuses (limited)
- **Manager**
  - Create tasks for any building in their company
  - Assign tasks to pros
  - Update and archive tasks
- **Pro**
  - Update assigned tasks only
- **Boss**
  - Full visibility, reporting, and configuration

### Documents
- **Manager/Boss**
  - Upload / update / delete documents
  - Configure expiry rules and alerts
- **Tenant**
  - View documents relevant to their building (read-only)

---

## Multi-Tenant Security Boundaries
EBM is designed as a multi-tenant system.
The primary security boundary is the **companyId**.

Backend rules:
- Every query for protected resources must filter by `companyId`
- Cross-company access is always denied
- Any “admin-like” capabilities are scoped to the user’s tenant

---

## Suggested Backend Pattern (Middleware + Guards)

### Auth Middleware
- `requireAuth` verifies JWT and sets `req.user`

### Role Guard
- `requireRole(['boss','manager'])` checks role

### Scope Guard (Ownership)
- `requireCompanyScope(resourceCompanyId)` ensures same tenant
- `requireBuildingAccess(buildingId)` checks membership/assignment
- `requireTaskAccess(taskId)` checks assignee/creator/manager permissions

> Even if implemented differently in code, this is the conceptual model the system follows.

---

## Security Notes / Best Practices
- Authorization is **server-side only** (never trust UI-only restrictions)
- Tokens should be stored securely (web: httpOnly cookie or careful localStorage strategy)
- Sensitive actions require strict RBAC checks and input validation
- Audit logging is recommended for critical changes (tasks status changes, deletions, payments)

---

## Summary
EBM uses:
- **JWT authentication** for identity
- **RBAC** for defining allowed actions per role
- **Ownership & tenant scoping** for real-world access rules

This combination ensures secure access across multiple apps while keeping business logic centralized and maintainable.
