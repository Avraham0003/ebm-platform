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
    NOTIF["Notifications Service\n(push + in-app)"]
    FILES["File Handling\nUploads & Document lifecycle"]
    PAY["Payments Integration\n(If enabled)"]
  end

  %% Data
  subgraph Data["Data Layer"]
    DB["MongoDB\n(Mongoose Schemas)"]
    STORAGE["Object Storage\n(Files/Images)"]
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
