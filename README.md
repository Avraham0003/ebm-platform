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
