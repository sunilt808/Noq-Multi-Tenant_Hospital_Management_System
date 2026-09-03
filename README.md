# 🏥 NOQ Hospital

### Smart Queue Management & Hospital Operations Platform

<p align="center">
  <strong>Digitizing the complete hospital journey — from appointment booking and queue management to consultation, billing, and hospital administration.</strong>
</p>

<p align="center">
  <a href="https://noq-hospital.vercel.app/signup">
    <img src="https://img.shields.io/badge/🚀%20Live%20Demo-2ea44f?style=for-the-badge" alt="Live Demo">
  </a>
  <a href="https://github.com/sunilt808/Noq-hospital">
    <img src="https://img.shields.io/badge/💻%20Source%20Code-181717?style=for-the-badge&logo=github&logoColor=white" alt="Source Code">
  </a>
  <a href="https://deepwiki.com/sunilt808/Noq-hospital">
    <img src="https://img.shields.io/badge/🧠%20DeepWiki-5B5BD6?style=for-the-badge" alt="DeepWiki">
  </a>
</p>

---

## 📌 Overview

**NOQ** is a full-stack hospital management platform built around a **Smart Queue Management System**.

Traditional hospitals often make patients deal with physical queues, uncertain waiting times, fragmented appointment processes, and disconnected administrative workflows.

NOQ brings these workflows into a single platform.

A patient can book an appointment, receive a digital token, monitor their queue position, attend a consultation, receive prescriptions, and manage bills — while doctors manage their consultation queues and hospital managers oversee infrastructure and revenue.

The platform also includes a **patient disciplinary system** for repeated appointment no-shows and a **role-based administration layer** for managing hospitals, users, operations, and audit activity.

---

# 🎯 Problem

A hospital visit involves much more than simply booking an appointment.

```text
Appointment
     ↓
Waiting
     ↓
Queue
     ↓
Doctor Consultation
     ↓
Prescription / Medical Record
     ↓
Billing
     ↓
Follow-up
```

When these workflows are disconnected, hospitals face:

* Long and unpredictable physical queues
* Poor visibility into patient waiting status
* Difficult appointment coordination
* Manual hospital administration
* Limited operational visibility
* No structured handling of repeated no-shows
* Fragmented financial and clinical workflows

**NOQ addresses these problems by connecting them into a single role-based system.**

---

# ✨ Core Features

## 🎟️ Smart Queue Management

The core of NOQ is its digital queue system.

Instead of relying on physical queues, every appointment can participate in a token-based workflow.

```text
Appointment
     ↓
Token Generated
     ↓
WAITING
     ↓
Doctor Calls Token
     ↓
CALLING
     ↓
Consultation
     ↓
COMPLETED
```

The backend queue service uses **atomic MongoDB updates** when generating token numbers, preventing duplicate token numbers during concurrent bookings.

Token state can transition through:

```text
waiting
   ↓
calling
   ↓
completed
```

with additional cancellation/missed states supported by the workflow.

When a doctor calls a patient, the system also triggers notifications so the patient knows that their turn has arrived.

---

# 📅 Appointment Management

Appointments form the central connection between patients and doctors.

The appointment system supports:

* Appointment creation
* Appointment status updates
* Patient appointment history
* Doctor appointment views
* Role-aware appointment filtering
* Token association
* Room and department context
* Audit logging for important updates

The backend serializes appointment data before sending it to the frontend, including normalization of token information and date/time representations.

---

# 👥 Role-Based Hospital System

NOQ is designed around **four distinct user roles**.

```text
                         NOQ
                          │
          ┌───────────────┼───────────────┐
          ↓               ↓               ↓
       PATIENT          DOCTOR           HM
          │               │               │
      Booking          Queue          Hospital
      Tokens         Consultation     Operations
      Records        Prescriptions    Revenue
      Billing        Patients         Staff
          │               │               │
          └───────────────┼───────────────┘
                          ↓
                        ADMIN
                          │
                  Platform Management
```

### 👤 Patient

Patients can:

* Register and authenticate
* Book appointments
* Track token/queue status
* View medical records
* View prescriptions
* Manage bills
* Receive notifications
* Submit reviews
* View their appointment history

### 👨‍⚕️ Doctor

Doctors can:

* View assigned appointments
* Manage consultation queues
* Call the next patient
* Complete consultations
* Issue prescriptions
* Manage patient records
* Review patient attendance
* Apply disciplinary actions for repeated no-shows

### 🏥 Hospital Manager

Hospital Managers oversee hospital-level operations:

* Departments
* Rooms
* Doctor assignments
* Hospital staff
* Revenue analytics
* Notifications
* Hospital-level audit activity

### 🛡️ System Admin

The platform administrator handles system-wide operations:

* Hospital registration approval
* Hospital lifecycle management
* Platform-level revenue
* Global audit activity
* Administrative settings

---

# 🔐 Authentication & Security

Security is implemented across both frontend and backend layers.

```text
                 Login
                   ↓
          Password Verification
                   ↓
              JWT Issued
                   ↓
       ┌───────────┴───────────┐
       ↓                       ↓
 React ProtectedRoutes    FastAPI Auth
       ↓                       ↓
 Role-Based UI          require_auth()
       └───────────┬───────────┘
                   ↓
              Protected API
```

### Password Security

Passwords are protected using:

* **PBKDF2**
* SHA-256
* 32-byte random salt
* 100,000 iterations

Passwords are stored as a salted hash representation rather than plaintext.

### JWT Authentication

NOQ uses stateless JWT authentication.

The token contains information including:

```text
sub   → User ID
email → User email
role  → User role
```

The backend validates the JWT signature using the configured secret and checks authentication claims before allowing protected operations.

### Role-Based Access Control

Authorization is enforced through:

* Frontend `ProtectedRoutes`
* Backend `require_auth` dependency
* Role-aware route behavior
* Hospital-specific access patterns

---

# 🚫 Patient Disciplinary System

One of NOQ's domain-specific features is its **three-level patient disciplinary workflow**.

Repeated appointment no-shows can disrupt both doctors and queue efficiency.

NOQ models this explicitly.

```text
              Active
                │
        1st Missed Appointment
                ↓
             Warned
                │
        2nd Missed Appointment
                ↓
              Fined
                │
        3rd Missed Appointment
                ↓
             Blocked
```

### Disciplinary Levels

| Level | Status    | Action                     |
| ----: | --------- | -------------------------- |
|     0 | `active`  | Normal booking access      |
|     1 | `warned`  | Warning + notification     |
|     2 | `fined`   | ₹200 fine recorded         |
|     3 | `blocked` | New appointments prevented |

The patient record tracks:

```text
blockLevel
missedAppointments
warnings
fines
status
```

The system also provides an unblock mechanism that can restore the patient's active state.

Every disciplinary action generates a notification so the patient is informed about their status.

---

# 💰 Billing & Revenue Management

NOQ connects financial operations with clinical appointments.

Rather than maintaining a completely independent billing source, bills are derived from appointment information.

```text
Completed Appointment
        ↓
     Consultation Fee
        ↓
     Bill Generation
        ↓
 Revenue Distribution
        ↓
 ┌──────┼────────┐
 ↓      ↓        ↓
Doctor   HM     Admin
15%     80%      5%
```

The recorded revenue distribution uses:

* 👨‍⚕️ Doctor — **15%**
* 🏥 Hospital Manager — **80%**
* 🛡️ Admin — **5%**

Revenue information is stored separately for distribution tracking and analytics.

### Revenue Analytics

Hospital Managers can view:

* Monthly revenue
* Total revenue
* Average transaction value
* Department-wise revenue

Administrators can view:

* Platform-wide revenue
* Doctor share
* Hospital share
* Admin share
* Hospital comparisons
* Patient statistics
* Hospital ratings

---

# 🩺 Medical Records & Prescriptions

Consultations connect directly to the clinical side of the platform.

```text
Appointment
     ↓
Doctor Consultation
     ↓
Medical Information
     ↓
Prescription
     ↓
Patient Medical Record
```

Doctors can create prescriptions containing medicine information and instructions linked to the relevant appointment.

Patients can then access their prescription and medical information through their portal.

---

# 🏥 Hospital Infrastructure

NOQ models the internal structure of hospitals rather than treating a hospital as a single entity.

```text
Hospital
   │
   ├── Departments
   │      └── Doctors
   │             └── Queues
   │
   └── Rooms
          └── Assigned Doctors
```

The platform manages:

* Hospitals
* Departments
* Rooms
* Doctor assignments
* Hospital staff
* Hospital status/lifecycle

Hospital registration follows an approval workflow where new hospital registrations can be reviewed by the System Admin.

---

# 🔔 Notification System

Notifications connect important events across the platform.

Examples include:

* Doctor calling a patient token
* Disciplinary warnings
* Fines
* Blocking events
* Operational hospital notifications
* Dashboard refresh events

```text
Business Event
      ↓
Notification Service
      ↓
Role / User Targeting
      ↓
User Notification Inbox
```

The queue service, for example, notifies patients when their token is called.

---

# 🧾 Audit Logging

NOQ includes an audit layer for tracking important system activity.

Authentication-related events such as:

* Login
* Signup
* Password reset
* Credential changes

are recorded through the audit system.

Important business operations, such as appointment status changes, can also be recorded.

This provides an additional layer of **accountability and traceability** for sensitive operations.

---

# 🏗️ System Architecture

NOQ follows a **decoupled client-server architecture**.

```text
                         ┌─────────────────────┐
                         │     React + Vite     │
                         │      SPA Frontend    │
                         └──────────┬──────────┘
                                    │
                              REST / JSON
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   FastAPI Backend   │
                         │      Uvicorn        │
                         └──────────┬──────────┘
                                    │
              ┌─────────────────────┼─────────────────────┐
              ↓                     ↓                     ↓
        Authentication        Business Routers       Services
              │                     │                     │
              │          ┌──────────┼──────────┐          │
              │          ↓          ↓          ↓          │
              │      Hospitals   Queues    Appointments  │
              │      Doctors     Tokens     Billing       │
              │      Patients    Revenue    Records       │
              │      Rooms       Reviews    Notifications │
              │                     │                     │
              └─────────────────────┼─────────────────────┘
                                    ↓
                              MongoDB Atlas
```

---

# 🧩 Backend Architecture

The backend is centered around a FastAPI application that aggregates **18 specialized routers**.

Major domains include:

```text
backend/
│
├── routes/
│   ├── auth.py
│   ├── users.py
│   ├── hospitals.py
│   ├── departments.py
│   ├── rooms.py
│   ├── doctors.py
│   ├── patients.py
│   ├── appointments.py
│   ├── queues.py
│   ├── tokens.py
│   ├── advanced_bookings.py
│   ├── bills.py
│   ├── revenue.py
│   ├── notifications.py
│   ├── prescriptions.py
│   ├── medical_records.py
│   ├── reviews.py
│   ├── complaints.py
│   └── ...
│
├── services/
│   ├── auth_service.py
│   ├── user_service.py
│   ├── queue_service.py
│   └── ...
│
├── database.py
├── main.py
└── init_mongo.py
```

The architecture separates API routing from reusable business logic.

For example:

```text
Queue Router
     ↓
Queue Service
     ↓
MongoDB Operations
```

This keeps queue-specific business rules out of individual API handlers.

---

# 🎟️ Queue Service — Technical Design

The queue service is one of the most important backend components.

### Token Generation

A naive implementation could:

```text
Read Current Token
      ↓
Increment Locally
      ↓
Write Token
```

This can create duplicate numbers when multiple requests happen concurrently.

NOQ instead uses MongoDB's atomic:

```text
find_one_and_update()
        +
       $inc
```

to increment the queue counters atomically before creating the token.

```text
Concurrent Requests
        │
        ▼
MongoDB Atomic Update
        │
   ┌────┴────┐
   ↓         ↓
Token 1    Token 2
   ↓         ↓
  T-001     T-002
```

This makes token generation safer under concurrent booking activity.

---

# 🧠 Frontend Architecture

The frontend is built as a React SPA with role-specific page organization.

```text
noq-frontend/
│
├── src/
│   ├── pages/
│   │   ├── patient/
│   │   ├── doctor/
│   │   ├── hm/
│   │   └── admin/
│   │
│   ├── services/
│   ├── hooks/
│   ├── context/
│   ├── components/
│   └── App.jsx
```

### Role-Based UI

```text
                    App
                     │
                Authentication
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
      Patient      Doctor       HM
       Portal      Portal      Portal
          │          │          │
          └──────────┼──────────┘
                     ↓
                   Admin
```

---

# 🔄 Frontend Service Layer

The frontend does not directly scatter API calls across every component.

Instead, domain-specific services handle communication with the backend.

```text
React Component
       ↓
Service Layer
       ↓
API Client
       ↓
JWT Authorization
       ↓
FastAPI Router
       ↓
Business Logic
       ↓
MongoDB
```

The service layer includes:

* Generic database/API operations
* Queue operations
* Authentication
* Advanced booking operations
* Data normalization
* Error handling
* Retry handling

The `useApiData` hook acts as a central data orchestrator for authenticated application state and provides role-aware helpers for commonly accessed data.

---

# 🗄️ Data Architecture

NOQ currently uses MongoDB as its primary operational datastore.

The project also contains a relational/SQLAlchemy layer for schema definitions and legacy/reference support.

```text
                    Application
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
         MongoDB Atlas           SQLite
        Primary Runtime       Schema / Legacy
           Storage               Support
```

### Core Entities

```text
User
Hospital
Department
Room
Appointment
Queue
Token
Advanced Booking
Revenue
Notification
Prescription
Medical Record
Review
Complaint
```

MongoDB collections support runtime operations such as:

```text
users
hospitals
appointments
queues
tokens
revenue_distributions
notifications
```

---

# 📡 API Domains

The FastAPI backend is organized by business responsibility.

| Domain               | Responsibility                      |
| -------------------- | ----------------------------------- |
| `/auth`              | Login, signup, authentication       |
| `/users`             | Profiles, user management           |
| `/hospitals`         | Hospital registration and lifecycle |
| `/appointments`      | Appointment lifecycle               |
| `/queues`            | Queue management                    |
| `/tokens`            | Token operations                    |
| `/advanced-bookings` | Advanced room/doctor allocation     |
| `/bills`             | Patient billing                     |
| `/revenue`           | Revenue distribution & analytics    |
| `/notifications`     | User notifications                  |
| `/prescriptions`     | Prescription management             |
| `/medical-records`   | Medical information                 |
| `/reviews`           | Reviews                             |
| `/complaints`        | Complaints                          |
| `/departments`       | Department management               |
| `/rooms`             | Room management                     |

---

# 🔁 End-to-End Patient Journey

A typical NOQ workflow looks like:

```text
1. Patient Registration
          ↓
2. Login
          ↓
3. Select Hospital
          ↓
4. Select Department / Doctor
          ↓
5. Book Appointment
          ↓
6. Token Generated
          ↓
7. Join Digital Queue
          ↓
8. Doctor Calls Token
          ↓
9. Patient Notification
          ↓
10. Consultation
          ↓
11. Prescription / Medical Record
          ↓
12. Billing
          ↓
13. Revenue Distribution
          ↓
14. Appointment Completed
```

This connects multiple subsystems around a single appointment lifecycle instead of treating each module independently.

---

# 🚀 Deployment

NOQ uses a separate deployment model for the frontend and backend.

```text
                  GitHub Repository
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
          Vercel                  Render
              │                     │
              ↓                     ↓
        React + Vite             FastAPI
        Frontend SPA             Backend API
                                    │
                                    ↓
                              MongoDB Atlas
```

### Frontend

Deployed using **Vercel**.

```text
npm run build
      ↓
    dist/
      ↓
   Vercel
```

The Vercel configuration also handles SPA rewrites required by React Router.

### Backend

Deployed using **Render**.

```text
Python 3.11
     ↓
Uvicorn
     ↓
FastAPI
     ↓
Render
```

The backend exposes a `/health` endpoint for service health monitoring.

---

# ⚙️ Local Development

## Prerequisites

* Python **3.11**
* Node.js
* npm
* MongoDB Atlas account

---

## 1. Clone Repository

```bash
git clone https://github.com/sunilt808/Noq-hospital.git
cd Noq-hospital
```

---

## 2. Backend Setup

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

---

## 3. Configure Environment Variables

Create:

```text
backend/.env
```

Example:

```env
MONGODB_URL=your_mongodb_connection_string
DATABASE_NAME=noq_hospital_db
JWT_SECRET=your_strong_secret
JWT_EXPIRATION_MINUTES=1440
FRONTEND_URL=http://localhost:5173
```

> Never commit real credentials or secrets to GitHub.

---

## 4. Initialize MongoDB

```bash
python init_mongo.py
```

The initialization script verifies the MongoDB connection before the application starts.

---

## 5. Start Backend

```bash
uvicorn main:app --host 127.0.0.1 --port 8000 --reload
```

Backend:

```text
http://127.0.0.1:8000
```

---

## 6. Frontend Setup

Open another terminal:

```bash
cd noq-frontend
npm install
```

Create:

```text
noq-frontend/.env
```

```env
VITE_API_URL=http://127.0.0.1:8000
```

Start the frontend:

```bash
npm run dev
```

Frontend:

```text
http://localhost:5173
```

---

# 🔑 Environment Variables

### Backend

| Variable                 | Purpose                    |
| ------------------------ | -------------------------- |
| `MONGODB_URL`            | MongoDB connection string  |
| `DATABASE_NAME`          | MongoDB database name      |
| `JWT_SECRET`             | JWT signing secret         |
| `JWT_EXPIRATION_MINUTES` | JWT lifetime               |
| `FRONTEND_URL`           | Frontend URL used for CORS |

### Frontend

| Variable       | Purpose             |
| -------------- | ------------------- |
| `VITE_API_URL` | FastAPI backend URL |

---

# 📊 Technical Highlights

| Area              | Implementation                        |
| ----------------- | ------------------------------------- |
| Frontend          | React + Vite SPA                      |
| Backend           | FastAPI + Uvicorn                     |
| Database          | MongoDB Atlas                         |
| Authentication    | JWT                                   |
| Password Security | PBKDF2 + SHA-256                      |
| Authorization     | RBAC                                  |
| Queue             | Atomic token generation               |
| Notifications     | Role/user-based notifications         |
| Billing           | Appointment-derived billing           |
| Revenue           | Stakeholder distribution + analytics  |
| Audit             | Credential & sensitive-action logging |
| Architecture      | Decoupled frontend/backend            |
| Deployment        | Vercel + Render                       |

---

# 🧪 Engineering Decisions

### Why MongoDB?

Hospital entities such as appointments, queues, notifications, and operational records require flexible document-oriented storage and rapid iteration.

### Why FastAPI?

FastAPI provides:

* Structured REST APIs
* Pydantic validation
* Dependency-based authentication
* Clear router separation
* High-performance ASGI execution

### Why a Service Layer?

Keeping business logic inside services such as the queue service prevents complex operations from becoming tightly coupled to HTTP route handlers.

### Why Atomic Token Generation?

Hospital queues can receive concurrent booking operations. Atomic counter updates prevent multiple requests from receiving the same token number.

---

# 🗺️ Future Improvements

Potential areas for further evolution include:

* Real-time WebSocket-based queue updates
* More granular tenant isolation
* Automated testing and CI pipelines
* Advanced appointment scheduling constraints
* Containerized deployment
* Observability and metrics
* More sophisticated notification delivery
* Expanded analytics and reporting

---

# 📸 Screenshots

> Add screenshots/GIFs of the actual application here.

Recommended screenshots:

```text
1. Landing / Login
2. Patient Dashboard
3. Appointment Booking
4. Live Queue
5. Doctor Dashboard
6. Token Management
7. Hospital Manager Dashboard
8. Revenue Analytics
9. Admin Dashboard
10. Medical Records / Prescription
```

Example:

```markdown
## 📸 Screenshots

### Patient Dashboard
![Patient Dashboard](screenshots/patient-dashboard.png)

### Doctor Queue
![Doctor Queue](screenshots/doctor-queue.png)

### Revenue Analytics
![Revenue Analytics](screenshots/revenue-dashboard.png)
```

---

# 🌐 Links

<p align="center">
  <a href="https://noq-hospital.vercel.app/signup">
    <img src="https://img.shields.io/badge/🚀%20Live%20Application-2ea44f?style=for-the-badge" alt="Live Application">
  </a>
  <a href="https://github.com/sunilt808/Noq-hospital">
    <img src="https://img.shields.io/badge/💻%20GitHub%20Repository-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Repository">
  </a>
  <a href="https://deepwiki.com/sunilt808/Noq-hospital">
    <img src="https://img.shields.io/badge/🧠%20DeepWiki%20Documentation-5B5BD6?style=for-the-badge" alt="DeepWiki Documentation">
  </a>
</p>

---

# 👨‍💻 Author

**Sunil T**

Computer Science & Engineering Undergraduate
Dayananda Sagar College of Engineering

<p>
  <a href="https://github.com/sunilt808">GitHub</a> ·
  <a href="https://leetcode.com/u/Sunil_T808/">LeetCode</a> ·
  <a href="https://www.geeksforgeeks.org/profile/suniltso14h">GeeksforGeeks</a>
</p>

---

<p align="center">
  <strong>NOQ — From appointment to consultation, without the queue chaos.</strong>
</p>
