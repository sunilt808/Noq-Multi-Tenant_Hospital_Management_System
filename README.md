<div align="center">

# 🏥 **NOQ**  
### Smart Queue Management & Hospital Operations Platform  

**Appointments → Tokens → Queues → Consultations → Records → Billing**  

<br/>

<img src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white" alt="React 19">
<img src="https://img.shields.io/badge/FastAPI-Backend-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
<img src="https://img.shields.io/badge/MongoDB-Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB Atlas">
<img src="https://img.shields.io/badge/JWT-Authentication-black?style=for-the-badge" alt="JWT">
<img src="https://img.shields.io/badge/RBAC-Authorization-6C47FF?style=for-the-badge" alt="RBAC">

<br/>

<a href="https://noq-hospital.vercel.app/signup"><img src="https://img.shields.io/badge/🚀%20Live%20Demo-2ea44f?style=for-the-badge&logo=vercel&logoColor=white" alt="Live Demo"></a>
<a href="https://github.com/sunilt808/Noq-hospital"><img src="https://img.shields.io/badge/💻%20Source%20Code-181717?style=for-the-badge&logo=github&logoColor=white" alt="Source Code"></a>
<a href="https://deepwiki.com/sunilt808/Noq-hospital"><img src="https://img.shields.io/badge/🧠%20DeepWiki-5B5BD6?style=for-the-badge" alt="DeepWiki"></a>

<br/><br/>

<div style="width: 80%; height: 2px; background: linear-gradient(90deg, #61DAFB, #009688, #47A248); margin: 0 auto;"></div>

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [The Problem](#-the-problem)
- [Core Features](#-core-features)
- [How NOQ Works](#-how-noq-works)
- [Smart Queue Engine](#-smart-queue-engine)
- [Priority & Location-Aware Booking](#-priority--location-aware-booking)
- [Role-Based System](#-role-based-system)
- [No-Show Management](#-no-show-management)
- [Billing & Revenue](#-billing--revenue)
- [Security](#-security)
- [Architecture](#-architecture)
- [Backend Design](#-backend-design)
- [Deployment](#-deployment)
- [Tech Stack](#-tech-stack)
- [Local Setup](#-local-setup)
- [Engineering Highlights](#-engineering-highlights)
- [Roadmap](#-roadmap)

---

## 📌 Overview

**NOQ** is a full-stack hospital management platform built around one central problem:

> *How can hospitals reduce unnecessary physical queues while keeping appointments, doctors, patients, records and billing connected?*

NOQ transforms the traditional hospital visit into a seamless digital workflow. Instead of treating appointments, queues, consultations, and billing as separate modules, the platform connects them around the **patient journey**.

```mermaid
flowchart LR
    A[📅 Appointment] --> B[🎟️ Digital Token]
    B --> C[⏳ Smart Queue]
    C --> D[👨‍⚕️ Consultation]
    D --> E[💊 Prescription]
    D --> F[📋 Medical Record]
    E --> G[💳 Billing]
    F --> G
    G --> H[💰 Revenue & Analytics]
```

---

## 🎯 The Problem

A typical hospital visit involves:

- ❌ **Long queues** & **uncertain waiting**
- ❌ **Manual coordination** & crowded premises
- ❌ **Disconnected workflows** (appointment → doctor → billing)
- ❌ **No-show appointments** & fragmented billing

NOQ addresses these as an **end-to-end workflow**, not just digitized forms.

```text
   LESS PHYSICAL WAITING
             ↓
   BETTER QUEUE CONTROL
             ↓
   BETTER PATIENT FLOW
             ↓
   BETTER HOSPITAL OPERATIONS
```

---

## ✨ Core Features

<div align="center">
<table>
<tr>
<td align="center" width="50%" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🎟️ **Smart Queue**  
  <small>Atomic token generation & lifecycle</small>
</td>
<td align="center" width="50%" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🏥 **Multi-Tenant**  
  <small>Multiple hospitals, one platform</small>
</td>
</tr>
<tr>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  📅 **Appointments**  
  <small>Booking & lifecycle management</small>
</td>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  👥 **Four Portals**  
  <small>Patient • Doctor • Manager • Admin</small>
</td>
</tr>
<tr>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🔐 **Secure Access**  
  <small>JWT + RBAC + protected routes</small>
</td>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🚫 **No-Show Control**  
  <small>Warn → Fine → Block escalation</small>
</td>
</tr>
<tr>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🩺 **Clinical Workflow**  
  <small>Consultations, prescriptions, records</small>
</td>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  💰 **Financial Workflow**  
  <small>Billing, revenue distribution, analytics</small>
</td>
</tr>
<tr>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🔔 **Notifications**  
  <small>Appointment & operational alerts</small>
</td>
<td align="center" style="padding: 15px; background: #f8f9fa; border-radius: 10px; border: 1px solid #dee2e6;">
  
  🧾 **Auditability**  
  <small>Traceability for critical actions</small>
</td>
</tr>
</table>
</div>

---

## 🧠 How NOQ Works

The platform connects four major actors:

```mermaid
flowchart TD
    P[👤 Patient] -->|Books| A[📅 Appointment]
    A -->|Generates| Q[⏳ Queue]
    Q -->|Token| D[👨‍⚕️ Doctor]
    D -->|Consult| M[📋 Medical Record]
    D -->|Prescribe| Rx[💊 Prescription]
    M --> B[💳 Billing]
    Rx --> B
    B --> Admin[🛡️ Admin / Revenue]
```

---

## 🎟️ Smart Queue Engine

A patient traverses a **stateful queue**:

```mermaid
stateDiagram-v2
    [*] --> Appointed
    Appointed --> Tokenized
    Tokenized --> Waiting
    Waiting --> Called
    Called --> Consultation
    Consultation --> Completed
    Completed --> [*]
```

### ⚙️ Atomic Token Generation

To avoid race conditions, NOQ uses **atomic MongoDB updates**:

```mermaid
sequenceDiagram
    participant A as Request A
    participant B as Request B
    participant DB as MongoDB
    A->>DB: find_one_and_update({$inc: {token: 1}})
    B->>DB: find_one_and_update({$inc: {token: 1}})
    DB-->>A: Token T-101
    DB-->>B: Token T-102
```

> 💡 **Key insight:** Token allocation is an **atomic database operation**, not a client-side counter.

---

## 📍 Priority & Location-Aware Booking

### 👴 Priority Advance Booking

| Priority Category | Criteria |
| :-- | :-- |
| 👴 **Senior Citizen** | Age ≥ 70 |
| 🤰 **Pregnant Patient** | — |
| 👶 **Child** | Age < 7 |

### 📍 500-Meter Normal Token Rule

```mermaid
flowchart LR
    subgraph Hospital[🏥 Hospital]
        H[Hospital]
    end
    subgraph Radius[500m radius]
        H
    end
    P1[📍 Patient ≤ 500m] -->|✅ Token Available| H
    P2[📍 Patient > 500m] -->|⏳ Wait until nearby| H
```

> **Purpose:** Reduce early arrivals, physical crowding, and traffic around hospital premises.

---

## 👥 Role-Based System

| Role | Responsibilities |
| :-- | :-- |
| 👤 **Patient** | Appointments, tokens, queue tracking, records, prescriptions, bills |
| 👨‍⚕️ **Doctor** | Queue management, consultations, prescriptions, records |
| 🏥 **Hospital Manager** | Departments, rooms, doctors, staff, revenue, analytics |
| 🛡️ **System Admin** | Hospital approval, platform management, revenue, audit |

### Workflow Summaries

**Patient:**  
`Register → Discover Hospital → Choose Doctor → Book → Token → Track → Consult → Prescription → Bill`

**Doctor:**  
`Login → View Appointments → Open Queue → Call Next → Consult → Prescribe → Record → Complete`

**Manager:**  
`Hospital → Departments → Doctors → Rooms → Staff → Appointments → Queues → Revenue`

---

## 🚫 No-Show Management

```mermaid
flowchart LR
    A[🟢 Active] -->|No-show #1| B[🟡 Warned]
    B -->|No-show #2| C[🟠 Fined]
    C -->|No-show #3| D[🔴 Blocked]
```

| Level | State | Effect |
| :-- | :-- | :-- |
| 0 | 🟢 Active | Normal booking |
| 1 | 🟡 Warned | Warning issued |
| 2 | 🟠 Fined | ₹200 fine recorded |
| 3 | 🔴 Blocked | New appointments prevented |

---

## 💰 Billing & Revenue

```mermaid
flowchart TD
    A[Appointment] --> B[Consultation]
    B --> C[Bill Generated]
    C --> D[Revenue Distribution]
    D --> E[Doctor 15%]
    D --> F[Hospital 80%]
    D --> G[Admin 5%]
```

---

## 🔐 Security

```mermaid
flowchart TD
    Login[Login] --> Password[Password Verification]
    Password --> JWT[JWT Issued]
    JWT --> Frontend[React Route Guard]
    JWT --> Backend[FastAPI Auth]
    Frontend --> RBAC[Role-Based UI]
    Backend --> Auth[Authorization]
    RBAC --> API[Protected API]
    Auth --> API
```

**Password Security:** PBKDF2, SHA-256, random salts, 100,000 iterations.  
**Authorization:** JWT + RBAC + protected routes + backend checks.

---

## 🧾 Auditability

All critical actions are logged:

```text
Authentication → Login, Signup, Password Reset, Credential Changes
Business Activity → Important status / workflow changes
```

---

## 🏢 Multi-Tenant Architecture

```mermaid
flowchart TD
    Platform[NOQ PLATFORM] --> H1[🏥 Hospital A]
    Platform --> H2[🏥 Hospital B]
    Platform --> H3[🏥 Hospital C]
    H1 --> D1[Departments]
    H2 --> D2[Departments]
    H3 --> D3[Departments]
    D1 --> Doc1[Doctors]
    D2 --> Doc2[Doctors]
    D3 --> Doc3[Doctors]
    Doc1 --> Q1[Queues]
    Doc2 --> Q2[Queues]
    Doc3 --> Q3[Queues]
```

---

## 🏗️ Architecture

```mermaid
flowchart TB
    subgraph Frontend
        React[React 19 + Vite]
    end
    subgraph Backend
        FastAPI[FastAPI]
        Auth[Authentication]
        Services[Business Services]
        Routers[Domain Routers]
    end
    subgraph Database
        Mongo[(MongoDB Atlas)]
    end
    React -->|REST API| FastAPI
    FastAPI --> Auth
    FastAPI --> Services
    FastAPI --> Routers
    Services --> Mongo
    Routers --> Mongo
```

---

## 🧩 Backend Design

```
backend/
│
├── routes/
│   ├── auth
│   ├── users
│   ├── hospitals
│   ├── departments
│   ├── rooms
│   ├── doctors
│   ├── patients
│   ├── appointments
│   ├── queues
│   ├── tokens
│   ├── advanced_bookings
│   ├── bills
│   ├── revenue
│   ├── notifications
│   ├── prescriptions
│   ├── medical_records
│   ├── reviews
│   ├── complaints
│   └── ...
│
├── services/
│
├── database.py
├── main.py
├── init_mongo.py
└── requirements.txt
```

**18+ domain routers** keep business areas separated.

---

## 🔄 Request Lifecycle

```mermaid
sequenceDiagram
    participant React as React Component
    participant Service as Frontend Service
    participant API as FastAPI Router
    participant Auth as Authentication
    participant RBAC as Authorization/RBAC
    participant DB as MongoDB

    React->>Service: User action
    Service->>API: HTTP Request
    API->>Auth: Validate JWT
    Auth-->>API: OK
    API->>RBAC: Check role
    RBAC-->>API: Allowed
    API->>DB: Query
    DB-->>API: Data
    API-->>Service: Response
    Service-->>React: UI update
```

---

## 🗄️ Core Data Model

```mermaid
erDiagram
    USER ||--o{ PATIENT : has
    USER ||--o{ DOCTOR : has
    USER ||--o{ MANAGER : has
    PATIENT ||--o{ HOSPITAL : visits
    DOCTOR ||--o{ HOSPITAL : works_at
    MANAGER ||--o{ HOSPITAL : manages
    HOSPITAL ||--o{ DEPARTMENT : has
    HOSPITAL ||--o{ ROOM : has
    HOSPITAL ||--o{ STAFF : has
    DEPARTMENT ||--o{ DOCTOR : employs
    DOCTOR ||--o{ APPOINTMENT : conducts
    APPOINTMENT ||--o{ TOKEN : generates
    APPOINTMENT ||--o{ QUEUE : joins
    APPOINTMENT ||--o{ MEDICAL_RECORD : creates
    APPOINTMENT ||--o{ PRESCRIPTION : creates
    APPOINTMENT ||--o{ BILL : has
    BILL ||--o{ REVENUE : contributes
```

---

## 🌐 API Domains

| Domain | Responsibility |
| :-- | :-- |
| Authentication | Signup, login and authentication |
| Users | User profiles |
| Hospitals | Hospital management |
| Departments | Department management |
| Rooms | Room management |
| Doctors | Doctor management |
| Patients | Patient management |
| Appointments | Appointment lifecycle |
| Queues | Queue operations |
| Tokens | Token operations |
| Advanced Bookings | Priority booking workflows |
| Bills | Billing |
| Revenue | Revenue and analytics |
| Notifications | Notifications |
| Prescriptions | Prescription management |
| Medical Records | Clinical records |
| Reviews | Reviews and ratings |
| Complaints | Complaint workflows |

---

## 🚀 Deployment

```mermaid
flowchart LR
    GitHub[GitHub Repo] --> Vercel[Vercel - Frontend]
    GitHub --> Render[Render - Backend]
    Render --> MongoDB[(MongoDB Atlas)]
```

### Production Stack

| Component | Platform |
| :-- | :-- |
| Frontend | Vercel |
| Backend | Render |
| Database | MongoDB Atlas |

---

## 🛠️ Tech Stack

**Languages:** Python 3.11+ • JavaScript • HTML • CSS  
**Frontend:** React 19 • Vite  
**Backend:** FastAPI • Python  
**Database:** MongoDB Atlas  
**DevOps:** Vercel • Docker • Kubernetes • Jenkins • Terraform  
**Security:** JWT • RBAC • PBKDF2

---

## ⚙️ Local Setup

### Prerequisites

- Python 3.11+
- Node.js
- npm
- MongoDB Atlas account

### Clone

```bash
git clone https://github.com/sunilt808/Noq-hospital.git
cd Noq-hospital
```

### Backend

```bash
cd backend
python -m venv venv
```

**Windows:** `venv\Scripts\activate`  
**Linux/macOS:** `source venv/bin/activate`

```bash
pip install -r requirements.txt
```

Create `backend/.env`:

```env
MONGODB_URL=your_mongodb_connection_string
DATABASE_NAME=noq_hospital_db
JWT_SECRET=your_secret
JWT_EXPIRATION_MINUTES=1440
FRONTEND_URL=http://localhost:5173
```

Initialize and run:

```bash
python init_mongo.py
uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

### Frontend

```bash
cd noq-frontend
npm install
```

Create `.env`:

```env
VITE_API_URL=http://127.0.0.1:8000
```

```bash
npm run dev
```

---

## 📈 Engineering Highlights

| Area | Implementation |
| :-- | :-- |
| 🎟️ Queue | Atomic token generation |
| 🏢 Architecture | Multi-tenant hospital platform |
| 🔐 Security | JWT + RBAC |
| 🔑 Passwords | PBKDF2 + SHA-256 |
| 📅 Workflow | Appointment-centered architecture |
| 🚫 Discipline | Warn → Fine → Block |
| 💰 Finance | Billing + revenue distribution |
| 🩺 Clinical | Records + prescriptions |
| 🔔 Communication | Notification workflows |
| 🧾 Traceability | Audit logging |
| 🌐 Deployment | Vercel + Render + MongoDB Atlas |
| 🧩 Backend | 18+ domain routers |

---

## 🗺️ Roadmap

### 🎯 Queue & Accessibility

- [ ] **Priority Advance Booking**
  - 👴 Senior citizens aged **70+**
  - 🤰 Pregnant patients
  - 👶 Children aged **under 7**

- [ ] **500-Meter Location-Based Normal Token**
  - Allow normal token generation within **500m of the hospital**
  - Reduce early arrivals, congestion, and traffic

```mermaid
flowchart TD
    LOC[📍 Patient Location] --> DIST{Distance Check}
    DIST -->|≤ 500m| OK[🎟️ Token Available]
    DIST -->|> 500m| WAIT[⏳ Wait until nearby]
```

### 🚀 DevOps & Production Engineering

- [ ] Dockerized production environment
- [ ] CI/CD automation
- [ ] Automated testing pipeline
- [ ] Environment-specific deployments
- [ ] Health checks
- [ ] Application monitoring & observability
- [ ] Scalable container orchestration

### 🔮 Future Platform Improvements

- [ ] Real-time queue updates
- [ ] Advanced appointment scheduling
- [ ] Expanded hospital analytics
- [ ] Stronger tenant isolation
- [ ] Improved scalability
- [ ] Mobile experience for patients and hospital staff

---

## 🌐 Explore NOQ

<div align="center">
  
  <a href="https://noq-hospital.vercel.app/signup"><img src="https://img.shields.io/badge/🚀%20Live%20Demo-2ea44f?style=for-the-badge&logo=vercel&logoColor=white" alt="Live Demo"></a>
  <a href="https://github.com/sunilt808/Noq-hospital"><img src="https://img.shields.io/badge/💻%20Source%20Code-181717?style=for-the-badge&logo=github&logoColor=white" alt="Source Code"></a>
  <a href="https://deepwiki.com/sunilt808/Noq-hospital"><img src="https://img.shields.io/badge/🧠%20DeepWiki-5B5BD6?style=for-the-badge" alt="DeepWiki"></a>

  <br/><br/>

  <div style="width: 60%; height: 2px; background: linear-gradient(90deg, #61DAFB, #009688, #47A248); margin: 0 auto;"></div>

  <br/>

  <h3>🏥 NOQ</h3>
  <em><strong>Don't wait in the queue. Know your place in it.</strong></em>  
  <small>Appointments → Tokens → Queues → Consultations → Records → Billing</small>

  <br/>

  <small>Built with ❤️ by **Sunil T**</small>

</div>
