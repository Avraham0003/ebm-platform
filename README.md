# 🏢 EBM – Easy Building Manager  
**Architecture & Platform Overview**

EBM (Easy Building Manager) is a **production SaaS platform** for managing residential buildings and maintenance companies.  
The system includes **Web, Android, and iOS applications** backed by a centralized REST API.

This repository provides a **technical overview and architecture documentation** of the platform.  
The full source code is split across multiple private repositories due to commercial usage.

---

## 🌐 System Architecture

```mermaid
flowchart TB
  %% Clients
  subgraph Clients["Clients"]
    U["EBM User App\n(Web + Android/iOS via Capacitor)"]
    M["EBM Manager App\n(Web + Android/iOS via Capacitor)"]
    S["EBM Site\n(Landing Page)"]
  end

  %% Backend
  subgraph Backend["Backend"]
    API["EBM Server\nNode.js + Express (REST)"]
    AUTH["Auth & RBAC\nJWT + Roles/Permissions"]
    JOBS["Schedulers / Background Jobs\n(Recurring tasks, reminders, expiries)"]
    NOTIF["Notifications Service\n(Push + In-app)"]
    FILES["File Handling\nUploads & Document lifecycle"]
    PAY["Payments Integration\n(Optional)"]
  end

  %% Data
  subgraph Data["Data Layer"]
    DB["MongoDB\n(Mongoose Schemas)"]
    STORAGE["Object Storage\n(Files / Images)"]
  end

  %% Connections
  U -->|HTTPS REST| API
  M -->|HTTPS REST| API
  S -->|Leads / Contact forms| API

  API --> AUTH
  API --> DB
  API --> FILES
  FILES --> STORAGE

  API --> NOTIF
  JOBS --> NOTIF
  JOBS --> DB

  API --> PAY
  PAY --> DB
  ```


## 🧠 Architecture Notes

Multi-client platform – Separate applications for tenants (User) and maintenance managers (Manager), all powered by a shared backend.

Central REST API – Built with Node.js and Express using MVC architecture.

Role-Based Access Control (RBAC) – Secure access using JWT authentication and role permissions.

Background Jobs – Handle recurring tasks, reminders, and document-expiry notifications.

File Management – Documents and images are stored in object storage and referenced from MongoDB.

## 🗂 Repository Purpose

This repository acts as an architecture and product showcase.

The actual system is implemented across multiple private repositories:

EBM User – Tenant-facing Web & Mobile application

EBM Manager – Maintenance management dashboard (Web & Mobile)

EBM Server – Backend API and core business logic

EBM Site – Marketing & landing page

## 🛠 Tech Stack Overview
Frontend

React

JavaScript (ES6+)

HTML5 / CSS3

Mobile

Capacitor (Android & iOS)

Backend

Node.js

Express

RESTful APIs

Database

MongoDB

Mongoose

Security & Architecture

JWT Authentication

Role-Based Access Control (RBAC)

MVC Architecture

Other

Background jobs & schedulers

File uploads & document lifecycle

Notifications system

Git version control

📸 Screenshots

(Add screenshots of the dashboard, mobile app, calendar, and tasks here)

## 👨‍💻 Author

Avraham Baranes
Full Stack Developer
Ashkelon, Israel

GitHub: https://github.com/Avraham0003

LinkedIn: https://linkedin.com/in/avraham-baranes-892b11245

## 📌 Notes

Some implementation details are omitted to protect proprietary business logic.
This repository focuses on architecture, patterns, and system design decisions rather than full source code.
