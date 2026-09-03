<div align="center">

# 🏥 NOQ

### Smart Queue Management & Hospital Operations Platform

**Appointments → Tokens → Queues → Consultations → Records → Billing**

<p>
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=for-the-badge&logo=react&logoColor=white" alt="React 19">
  <img src="https://img.shields.io/badge/FastAPI-Backend-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
  <img src="https://img.shields.io/badge/MongoDB-Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white" alt="MongoDB Atlas">
  <img src="https://img.shields.io/badge/JWT-Authentication-black?style=for-the-badge" alt="JWT">
  <img src="https://img.shields.io/badge/RBAC-Authorization-6C47FF?style=for-the-badge" alt="RBAC">
</p>

<p>
  <a href="https://noq-hospital.vercel.app/signup">
    <img src="https://img.shields.io/badge/🚀%20LIVE%20DEMO-2ea44f?style=for-the-badge" alt="Live Demo">
  </a>
  <a href="https://github.com/sunilt808/Noq-hospital">
    <img src="https://img.shields.io/badge/💻%20SOURCE%20CODE-181717?style=for-the-badge&logo=github&logoColor=white" alt="Source Code">
  </a>
  <a href="https://deepwiki.com/sunilt808/Noq-hospital">
    <img src="https://img.shields.io/badge/🧠%20DEEPWIKI-5B5BD6?style=for-the-badge" alt="DeepWiki">
  </a>
</p>

</div>

---

## 🧭 Navigation

* [Overview](#-overview)
* [The Problem](#-the-problem)
* [Core Features](#-core-features)
* [How NOQ Works](#-how-noq-works)
* [Smart Queue Engine](#-smart-queue-engine)
* [Priority & Location-Aware Booking](#-priority--location-aware-booking)
* [Role-Based System](#-role-based-system)
* [No-Show Management](#-no-show-management)
* [Billing & Revenue](#-billing--revenue)
* [Security](#-security)
* [Architecture](#️-architecture)
* [Backend Design](#-backend-design)
* [Deployment](#-deployment)
* [Tech Stack](#️-tech-stack)
* [Local Setup](#️-local-setup)
* [Engineering Highlights](#-engineering-highlights)
* [Roadmap](#-roadmap)

---

# 📌 Overview

**NOQ** is a full-stack hospital management platform designed around one central problem:

> **How can hospitals reduce unnecessary physical queues while keeping appointments, doctors, patients, records and billing connected?**

NOQ turns the traditional hospital visit into a digital workflow.

Instead of treating appointments, queues, consultations and billing as separate CRUD modules, the system connects them around the **patient journey**.

```text
                           🏥 NOQ

     📅 Appointment
           │
           ▼
      🎟️ Digital Token
           │
           ▼
       ⏳ Smart Queue
           │
           ▼
     👨‍⚕️ Consultation
        ┌──┴──┐
        ▼     ▼
   💊 Rx    📋 Medical Record
        │     │
        └──┬──┘
           ▼
        💳 Billing
           │
           ▼
     💰 Revenue & Analytics
```

---

# 🎯 The Problem

A typical hospital visit can involve:

```text
Long queues
     +
Uncertain waiting time
     +
Manual coordination
     +
Crowded hospital premises
     +
Disconnected patient/doctor workflows
     +
No-show appointments
     +
Fragmented billing
```

NOQ approaches the problem as an **end-to-end workflow**, rather than simply digitizing forms.

### The goal

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

# ✨ Core Features

<table>
<tr>
<td width="50%">

## 🎟️ Smart Queue

Digital token generation and queue lifecycle management.

</td>

<td width="50%">

## 🏥 Multi-Tenant

Multiple hospitals can operate through a shared platform.

</td>
</tr>

<tr>
<td>

## 📅 Appointments

Appointment booking and lifecycle management.

</td>

<td>

## 👥 Four Portals

Patient, Doctor, Hospital Manager and System Admin workflows.

</td>
</tr>

<tr>
<td>

## 🔐 Secure Access

JWT authentication + RBAC + protected routes.

</td>

<td>

## 🚫 No-Show Control

Warn → Fine → Block escalation system.

</td>
</tr>

<tr>
<td>

## 🩺 Clinical Workflow

Consultations, prescriptions and medical records.

</td>

<td>

## 💰 Financial Workflow

Billing, revenue distribution and analytics.

</td>
</tr>

<tr>
<td>

## 🔔 Notifications

Important appointment and operational events.

</td>

<td>

## 🧾 Auditability

Traceability for important system activities.

</td>
</tr>
</table>

---

# 🧠 How NOQ Works

The platform connects four major actors:

```text
                    ┌───────────────┐
                    │    PATIENT    │
                    └───────┬───────┘
                            │
                     Appointment
                            │
                            ▼
                    ┌───────────────┐
                    │     QUEUE     │
                    └───────┬───────┘
                            │
                         Token
                            │
                            ▼
                    ┌───────────────┐
                    │    DOCTOR     │
                    └───────┬───────┘
                            │
                       Consultation
                            │
                 ┌──────────┴──────────┐
                 ▼                     ▼
          Medical Record          Prescription
                 │                     │
                 └──────────┬──────────┘
                            ▼
                         Billing
                            │
                            ▼
                     Revenue / Admin
```

---

# 🎟️ Smart Queue Engine

The queue is the core operational component of NOQ.

A patient can move through a controlled queue lifecycle:

```text
┌───────────┐
│ APPOINTED │
└─────┬─────┘
      ↓
┌───────────┐
│ TOKENIZED │
└─────┬─────┘
      ↓
┌───────────┐
│  WAITING  │
└─────┬─────┘
      ↓
┌───────────┐
│  CALLED   │
└─────┬─────┘
      ↓
┌──────────────┐
│ CONSULTATION │
└──────┬───────┘
       ↓
┌──────────────┐
│   COMPLETED  │
└──────────────┘
```

## ⚙️ Atomic Token Generation

A naive implementation could perform:

```text
READ counter
     ↓
counter + 1
     ↓
WRITE counter
```

Under concurrent requests, this can produce duplicate or conflicting values.

NOQ instead uses an atomic MongoDB update:

```text
              Concurrent Requests
                 │          │
                 ▼          ▼
              Request A   Request B
                 │          │
                 └────┬─────┘
                      ▼
             find_one_and_update()
                      │
                     $inc
                      │
                      ▼
                Atomic Counter
                  │         │
                  ▼         ▼
                T-101     T-102
```

The important part is that token allocation is handled as an **atomic database operation**, rather than a client-side counter.

---

# 📍 Priority & Location-Aware Booking

One of the planned extensions of NOQ is to make queue access more sensitive to **patient needs and physical location**.

## 👴 Priority Advance Booking

Dedicated advance-booking support is planned for patients who may benefit from reduced physical waiting:

```text
Priority Eligibility

👴 Senior Citizen
   Age ≥ 70

        OR

🤰 Pregnant Patient

        OR

👶 Child
   Age < 7
```

The goal is to provide a dedicated appointment path instead of forcing these patients into the same access pattern as normal walk-in traffic.

---

## 📍 500-Meter Normal Token Rule

For normal token booking, NOQ is planned to use a **500-meter geofence around the hospital**.

```text
                 🏥 HOSPITAL
                      ◎
                 500m radius
              ╭──────────────╮
            ╭─┤              ├─╮
           │  │   BOOKING    │  │
           │  │    ZONE      │  │
           │  │              │  │
            ╰─┤              ├─╯
              ╰──────────────╯

          ≤ 500m        > 500m
             │             │
             ▼             ▼
        ✅ Token       ❌ Wait
        available     until nearby
```

### Why?

The intention is to reduce:

* unnecessary early arrivals
* physical crowding
* queue congestion
* traffic around hospital premises

while still allowing patients who are actually nearby to obtain a normal token.

> **The system is designed around the idea that patients should not have to stand in a physical queue just to participate in a queue.**

---

# 👥 Role-Based System

NOQ provides four distinct application experiences.

| Role                    | Main Responsibilities                                               |
| ----------------------- | ------------------------------------------------------------------- |
| 👤 **Patient**          | Appointments, tokens, queue tracking, records, prescriptions, bills |
| 👨‍⚕️ **Doctor**        | Queue management, consultations, prescriptions, medical records     |
| 🏥 **Hospital Manager** | Departments, rooms, doctors, staff, revenue, analytics              |
| 🛡️ **System Admin**    | Hospital approval, platform management, revenue, audit activity     |

---

# 👤 Patient Workflow

```text
Register
   ↓
Discover Hospital
   ↓
Choose Department / Doctor
   ↓
Book Appointment
   ↓
Receive Token
   ↓
Track Queue
   ↓
Doctor Consultation
   ↓
Prescription / Medical Record
   ↓
Billing
```

Patients can manage their complete hospital journey from a single portal.

---

# 👨‍⚕️ Doctor Workflow

```text
Doctor Login
     ↓
View Today's Appointments
     ↓
Open Queue
     ↓
Call Next Patient
     ↓
Consult
     ↓
Prescription
     ↓
Medical Record
     ↓
Complete Appointment
```

---

# 🏥 Hospital Manager Workflow

```text
Hospital
   │
   ├── Departments
   ├── Doctors
   ├── Rooms
   ├── Staff
   ├── Appointments
   ├── Queues
   └── Revenue
```

Hospital managers receive operational visibility instead of relying entirely on manual coordination.

---

# 🚫 No-Show Management

Repeated no-shows affect doctor availability and hospital capacity.

NOQ models this as an escalation workflow:

```text
                         ┌──────────┐
                         │  ACTIVE  │
                         └────┬─────┘
                              │
                         No-show #1
                              ↓
                         ┌──────────┐
                         │ ⚠️ WARN  │
                         └────┬─────┘
                              │
                         No-show #2
                              ↓
                         ┌──────────┐
                         │ 💰 FINE  │
                         └────┬─────┘
                              │
                         No-show #3
                              ↓
                         ┌──────────┐
                         │ 🚫 BLOCK │
                         └──────────┘
```

| Level | State      | Effect                     |
| ----: | ---------- | -------------------------- |
|     0 | 🟢 Active  | Normal booking             |
|     1 | 🟡 Warned  | Warning issued             |
|     2 | 🟠 Fined   | ₹200 fine recorded         |
|     3 | 🔴 Blocked | New appointments prevented |

---

# 💰 Billing & Revenue

NOQ connects clinical completion with financial operations.

```text
Appointment
     ↓
Consultation
     ↓
Bill Generated
     ↓
Revenue Distribution
     │
     ├───────────────┐
     ↓               ↓
 Doctor           Hospital
  15%               80%
     \               /
      \             /
       └─────┬─────┘
             ↓
          Admin
           5%
```

Hospital managers can monitor operational and financial information such as:

* Revenue
* Monthly performance
* Department revenue
* Average transaction value

Platform administrators can monitor:

* Platform revenue
* Hospital performance
* Doctor share
* Hospital share
* Patient statistics
* Ratings

---

# 🩺 Clinical Workflow

Consultations are connected directly to patient clinical information.

```text
Appointment
     ↓
Consultation
     ├───────────────┐
     ↓               ↓
Medical Record   Prescription
     │               │
     └───────┬───────┘
             ↓
       Patient History
```

This keeps the clinical information associated with the actual appointment rather than treating it as an unrelated record.

---

# 🔔 Notifications

Important system events can generate notifications.

```text
Business Event
      │
      ▼
Notification Service
      │
      ├── Appointment Updates
      ├── Token / Queue Events
      ├── Warnings
      ├── Fines
      ├── Blocking Events
      └── Hospital Updates
```

---

# 🔐 Security

Security is enforced across the application stack.

```text
                     LOGIN
                       │
                       ▼
              Password Verification
                       │
                       ▼
                  JWT Issued
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
      React Route Guard      FastAPI Auth
             │                   │
             ▼                   ▼
       Role-Based UI        Authorization
             │                   │
             └─────────┬─────────┘
                       ▼
                 Protected API
```

### 🔑 Password Security

NOQ uses:

* PBKDF2
* SHA-256
* Random salts
* 100,000 iterations

### 🛡️ Authorization

Access is controlled using:

```text
JWT Authentication
       +
Role-Based Access Control
       +
Protected Frontend Routes
       +
Backend Authorization
```

---

# 🧾 Auditability

Important system operations are designed to remain traceable.

```text
Authentication
     │
     ├── Login
     ├── Signup
     ├── Password Reset
     └── Credential Changes

Business Activity
     │
     └── Important status / workflow changes
```

This helps with operational investigation and accountability.

---

# 🏢 Multi-Tenant Architecture

NOQ is designed around a shared hospital platform.

```text
                         NOQ PLATFORM
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
        🏥 Hospital A    🏥 Hospital B    🏥 Hospital C
             │                │                │
        Departments      Departments      Departments
             │                │                │
          Doctors          Doctors          Doctors
             │                │                │
           Queues           Queues           Queues
```

Each hospital maintains its own operational context while platform-level administration remains available to the system administrator.

---

# 🏗️ Architecture

NOQ follows a decoupled frontend/backend architecture.

```text
                         ┌──────────────────────┐
                         │     React 19 + Vite  │
                         │   Role-Based Client   │
                         └───────────┬──────────┘
                                     │
                                  REST API
                                     │
                                     ▼
                         ┌──────────────────────┐
                         │       FastAPI        │
                         │   Domain-Based API   │
                         └───────────┬──────────┘
                                     │
             ┌───────────────────────┼───────────────────────┐
             │                       │                       │
             ▼                       ▼                       ▼
      Authentication          Business Services       Domain Routers
             │                       │                       │
             │                ┌──────┼──────┐                │
             │                ▼      ▼      ▼                │
             │             Queue   Billing  Booking          │
             │                │      │      │                │
             └────────────────┼──────┼──────┼────────────────┘
                              │      │      │
                              └──────┼──────┘
                                     ▼
                              MongoDB Atlas
```

---

# 🧩 Backend Design

The backend is organized around domain-specific routers instead of putting the entire application into one large API module.

```text
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

The backend contains **18+ domain routers**, keeping major business areas separated.

---

# 🔄 Request Lifecycle

A typical authenticated request follows:

```text
React Component
      ↓
Frontend Service
      ↓
HTTP Request
      ↓
FastAPI Router
      ↓
Authentication
      ↓
Authorization / RBAC
      ↓
Business Service
      ↓
MongoDB Atlas
      ↓
Response
      ↓
React UI
```

---

# 🗄️ Core Data Model

```text
                         USER
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
       PATIENT          DOCTOR         MANAGER
          │               │               │
          └───────────────┼───────────────┘
                          │
                       HOSPITAL
                          │
             ┌────────────┼────────────┐
             ▼            ▼            ▼
        Department      Room        Staff
             │
           Doctor
             │
             ▼
        Appointment
             │
       ┌─────┼─────┬──────────────┐
       ▼     ▼     ▼              ▼
     Token  Queue  Record    Prescription
                         │
                         ▼
                        Bill
                         │
                         ▼
                      Revenue
```

---

# 🌐 API Domains

| Domain            | Responsibility                   |
| ----------------- | -------------------------------- |
| Authentication    | Signup, login and authentication |
| Users             | User profiles                    |
| Hospitals         | Hospital management              |
| Departments       | Department management            |
| Rooms             | Room management                  |
| Doctors           | Doctor management                |
| Patients          | Patient management               |
| Appointments      | Appointment lifecycle            |
| Queues            | Queue operations                 |
| Tokens            | Token operations                 |
| Advanced Bookings | Priority booking workflows       |
| Bills             | Billing                          |
| Revenue           | Revenue and analytics            |
| Notifications     | Notifications                    |
| Prescriptions     | Prescription management          |
| Medical Records   | Clinical records                 |
| Reviews           | Reviews and ratings              |
| Complaints        | Complaint workflows              |

---

# 🚀 Deployment

```text
                           GitHub
                             │
                    ┌────────┴────────┐
                    ▼                 ▼
                 Vercel             Render
                    │                 │
                    ▼                 ▼
              React + Vite          FastAPI
                                      │
                                      ▼
                                MongoDB Atlas
```

### Production Stack

| Component | Platform      |
| --------- | ------------- |
| Frontend  | Vercel        |
| Backend   | Render        |
| Database  | MongoDB Atlas |

---

# 🛠️ Tech Stack

### Languages

<p>
  <img src="https://skillicons.dev/icons?i=python,javascript,html,css" alt="Languages">
</p>

### Frontend

<p>
  <img src="https://skillicons.dev/icons?i=react,vite" alt="Frontend">
</p>

### Backend

<p>
  <img src="https://skillicons.dev/icons?i=fastapi,python" alt="Backend">
</p>

### Database

<p>
  <img src="https://skillicons.dev/icons?i=mongodb" alt="Database">
</p>

### Engineering

```text
REST APIs
JWT
RBAC
Multi-Tenant Architecture
Atomic Database Operations
Service Layer
Domain-Based Routing
```

### Deployment

<p>
  <img src="https://skillicons.dev/icons?i=vercel,docker,kubernetes,jenkins,terraform" alt="DevOps">
</p>

---

# ⚙️ Local Setup

## Prerequisites

* Python 3.11+
* Node.js
* npm
* MongoDB Atlas account

## Clone

```bash
git clone https://github.com/sunilt808/Noq-hospital.git
cd Noq-hospital
```

## Backend

```bash
cd backend

python -m venv venv
```

### Windows

```bash
venv\Scripts\activate
```

### Linux / macOS

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create:

```text
backend/.env
```

```env
MONGODB_URL=your_mongodb_connection_string
DATABASE_NAME=noq_hospital_db
JWT_SECRET=your_secret
JWT_EXPIRATION_MINUTES=1440
FRONTEND_URL=http://localhost:5173
```

Initialize:

```bash
python init_mongo.py
```

Run:

```bash
uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

---

## Frontend

Open another terminal:

```bash
cd noq-frontend
npm install
```

Create:

```text
.env
```

```env
VITE_API_URL=http://127.0.0.1:8000
```

Run:

```bash
npm run dev
```

---

# 📈 Engineering Highlights

<div align="center">

| Area             | Implementation                    |
| ---------------- | --------------------------------- |
| 🎟️ Queue        | Atomic token generation           |
| 🏢 Architecture  | Multi-tenant hospital platform    |
| 🔐 Security      | JWT + RBAC                        |
| 🔑 Passwords     | PBKDF2 + SHA-256                  |
| 📅 Workflow      | Appointment-centered architecture |
| 🚫 Discipline    | Warn → Fine → Block               |
| 💰 Finance       | Billing + revenue distribution    |
| 🩺 Clinical      | Records + prescriptions           |
| 🔔 Communication | Notification workflows            |
| 🧾 Traceability  | Audit logging                     |
| 🌐 Deployment    | Vercel + Render + MongoDB Atlas   |
| 🧩 Backend       | 18+ domain routers                |

</div>

---

# 🗺️ Roadmap

NOQ's next phase focuses on **queue intelligence, accessibility and production engineering**.

## 🎯 Queue & Accessibility

* [ ] **Priority Advance Booking**

  * 👴 Senior citizens aged **70+**
  * 🤰 Pregnant patients
  * 👶 Children aged **under 7**

* [ ] **500-Meter Location-Based Normal Token**

  * Allow normal token generation within **500m of the hospital**
  * Reduce unnecessary early arrivals
  * Reduce physical queue congestion
  * Improve patient flow around hospitals

```text
              📍 PATIENT LOCATION
                       │
                       ▼
                Distance Check
                       │
                ┌──────┴──────┐
                ▼             ▼
             ≤ 500m         > 500m
                │             │
                ▼             ▼
           🎟️ TOKEN       ⏳ WAIT
           AVAILABLE      UNTIL NEARBY
```

## 🚀 DevOps & Production Engineering

* [ ] Dockerized production environment
* [ ] CI/CD automation
* [ ] Automated testing pipeline
* [ ] Environment-specific deployments
* [ ] Health checks
* [ ] Application monitoring
* [ ] Production observability
* [ ] Scalable container orchestration

## 🔮 Future Platform Improvements

* [ ] Real-time queue updates
* [ ] Advanced appointment scheduling
* [ ] Expanded hospital analytics
* [ ] Stronger tenant isolation
* [ ] Improved scalability
* [ ] Mobile experience for patients and hospital staff

---

# 🌐 Explore NOQ

<div align="center">

<a href="https://noq-hospital.vercel.app/signup">
<img src="https://img.shields.io/badge/🚀%20TRY%20THE%20LIVE%20APPLICATION-2ea44f?style=for-the-badge" alt="Live Application">
</a>

<a href="https://github.com/sunilt808/Noq-hospital">
<img src="https://img.shields.io/badge/💻%20EXPLORE%20SOURCE%20CODE-181717?style=for-the-badge&logo=github&logoColor=white" alt="Source Code">
</a>

<a href="https://deepwiki.com/sunilt808/Noq-hospital">
<img src="https://img.shields.io/badge/🧠%20EXPLORE%20ARCHITECTURE-5B5BD6?style=for-the-badge" alt="DeepWiki">
</a>

</div>

---

<div align="center">

## 🏥 NOQ

### **Don't wait in the queue. Know your place in it.**

Appointments → Tokens → Queues → Consultations → Records → Billing

**Built with ❤️ by Sunil T**

</div>
