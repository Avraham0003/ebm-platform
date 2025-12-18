# System Architecture – EBM Platform

## Overview
EBM (Easy Building Manager) is a multi-tenant SaaS platform designed to manage residential buildings and maintenance operations.

The system is composed of multiple client applications (Web & Mobile) that communicate with a centralized backend via REST APIs.  
All business logic, security, and data management are handled on the server side.

---

## High-Level Architecture

The platform follows a **client-server architecture** with a clear separation of concerns:

- **Clients:** User-facing applications (Tenant, Manager, Marketing site)
- **Backend:** Central API responsible for business logic
- **Data Layer:** Persistent storage for structured data and files

---

## Clients Layer

### EBM User App
- Target users: Tenants
- Platforms: Web, Android, iOS
- Responsibilities:
  - View tasks and notifications
  - Submit requests
  - Access documents and updates

### EBM Manager App
- Target users: Maintenance managers / company staff
- Platforms: Web, Android, iOS
- Responsibilities:
  - Manage buildings, users, and tasks
  - Assign work and monitor progress
  - Access reports and system data

### EBM Site
- Public-facing landing page
- Handles marketing content and lead generation

---

## Backend Layer

### API Server
- Built with Node.js and Express
- RESTful API design
- MVC architecture for maintainability and scalability

### Authentication & Authorization
- JWT-based authentication
- Role-Based Access Control (RBAC)
- Fine-grained permissions per role

### Business Logic
- Centralized validation and rules enforcement
- Shared logic across all client platforms

### Background Processing
- Scheduled jobs for:
  - Recurring tasks
  - Notifications
  - Document expiration checks

---

## Data Layer

### Database
- MongoDB as the primary database
- Mongoose ODM for schema modeling
- Optimized for flexible and evolving data structures

### File Storage
- External object storage for documents and images
- Database stores references and metadata only

---

## Scalability & Maintainability

- Stateless API design
- Clear separation between layers
- Easy to extend with new modules and clients
- Designed to support multiple companies and buildings

---

## Summary

The EBM architecture emphasizes:
- Clear responsibility boundaries
- Security-first design
- Scalability for real-world usage
- Single backend serving multiple platforms
