# ⚖️ Social Work Case Management System

A full-stack web application for managing social work cases within a legal consultancy (Consultorios Jurídicos). Built with **Vue 3** on the frontend and **Node.js / Express** on the backend, using **Sequelize ORM** with a relational database.

---

## 📋 Overview

This system digitizes and streamlines the entire social work workflow: from case intake and family group registration to appointment scheduling, status tracking, and report generation. It was designed for use by social workers and legal staff in a law clinic environment.

---

## ✨ Features

- 📁 **Case Management** — Create, view, update, and soft-delete social work cases linked to initial consultations
- 👨‍👩‍👧 **Living Group Registry** — Register and manage members of a client's household (relationships, ages, occupations)
- 📅 **Appointment Scheduling** — Visual schedule management with status indicators (Al Día / Alerta / Crítico)
- 🔄 **Status Workflow** — Cases flow through statuses: `Activo → Archivado`, with automatic downstream updates to related records
- 📊 **Excel Report Export** — Generate filtered Excel reports by date range using ExcelJS
- 📄 **Word Document Export** — Generate per-case `.docx` reports using the `docx` library
- 🔍 **Search & Filtering** — Search cases by name, ID number, or status; dynamic filter buttons
- 🔐 **Audit Trail** — Every INSERT / UPDATE / DELETE is logged to an audit table with the internal user ID
- 🔒 **Auth Middleware** — Protected routes for case creation

---

## 🗂️ Project Structure

```
├── frontend/
│   ├── SocialWorkCases.vue        # Case list with search, filters, and status badges
│   ├── SocialWorkNewCase.vue      # Full case intake & edit form
│   └── SocialWorkSchedule.vue    # Appointment dashboard with statistics
│
├── backend/
│   ├── schemas/
│   │   ├── Social_Work.js         # Sequelize model — social work cases
│   │   └── Living_Group.js        # Sequelize model — household members
│   │
│   ├── models/
│   │   ├── Social_WorkModel.js    # Business logic, Excel/Word report generation
│   │   └── Living_GroupModel.js   # CRUD + audit logging for living groups
│   │
│   ├── controllers/
│   │   ├── SocialWorkController.js
│   │   └── LivingGroupController.js
│   │
│   └── routes/
│       ├── social_work_routes.js
│       └── living_group_routes.js
```

---

## 🛠️ Tech Stack

| Layer       | Technology                              |
|-------------|------------------------------------------|
| Frontend    | Vue 3, Tailwind CSS                      |
| Backend     | Node.js, Express.js                      |
| ORM         | Sequelize                                |
| Database    | MySQL / PostgreSQL (relational)          |
| Reports     | ExcelJS (`.xlsx`), docx (`.docx`)        |
| Auth        | JWT middleware                           |
| Date/Time   | moment-timezone (GMT-5 / Guayaquil)      |

---

## 🔌 API Endpoints

### Social Work

| Method | Endpoint                          | Description                              |
|--------|-----------------------------------|------------------------------------------|
| GET    | `/social-work`                    | Get all social work records              |
| GET    | `/social-work/:id`                | Get record by process number             |
| GET    | `/social-work/user/:initCode`     | Get user linked to a social work case    |
| POST   | `/social-work`                    | Create new case *(auth required)*        |
| PUT    | `/social-work/:id`                | Update case data                         |
| PUT    | `/social-work/status/:id`         | Update case status                       |
| DELETE | `/social-work/:id`                | Soft delete a case                       |
| GET    | `/social-work/report/excel`       | Download Excel report (`?startDate=&endDate=`) |
| GET    | `/social-work/report/word`        | Download Word report (`?processNumber=`) |

### Living Groups

| Method | Endpoint                                    | Description                          |
|--------|---------------------------------------------|--------------------------------------|
| GET    | `/living-groups`                            | Get all living group members         |
| GET    | `/living-groups/:id`                        | Get member by ID                     |
| GET    | `/living-groups/process/:processNumber`     | Get all members for a case           |
| POST   | `/living-groups`                            | Add a member                         |
| PUT    | `/living-groups/:id`                        | Update a member                      |
| DELETE | `/living-groups/:id`                        | Delete a member                      |

---

## 🗃️ Data Model Highlights

### `Social_Work`
- Primary key: `SW_ProcessNumber` (string)
- Linked to `InitialConsultations` via `Init_Code` (one-to-one)
- Tracks: user requests, violence episodes, disability info, housing type, income, counterpart data, and more
- Status field with soft-delete pattern

### `LivingGroup`
- Auto-incremented `LG_LivingGroup_ID`
- Foreign key `SW_ProcessNumber` → `Social_Work` (cascade delete)
- Fields: name, age, relationship, occupation, notes

---

## 🚀 Getting Started

### Prerequisites

- Node.js v18+
- MySQL or PostgreSQL
- npm or yarn

## 📸 Key Screens

| Screen | Description |
|--------|-------------|
| **Case List** | Grid of all cases filterable by status (Activo, En Proceso, Archivado) and searchable by name or ID |
| **Case Form** | Multi-section intake form covering case info, requests, disability, housing, counterpart data, and living group |
| **Schedule** | Appointment management dashboard with stats cards (Total, Al Día, Alerta, Crítico) |

---

## 🔍 Notable Implementation Details

- **Timezone-aware reports**: Excel exports convert user-input dates from `America/Guayaquil` (GMT-5) to UTC before querying, preventing off-by-one-day errors
- **Audit logging**: Every data mutation records the `internalUser` ID, operation type, table name, and a human-readable description
- **Archived case lock**: When a case status is set to `Archivado`, the UI locks the entire form and the backend resets the related `Init_Type` if `Init_MandatorySW` was true
- **Graceful null handling**: Numeric fields (age, income, disability %) convert empty strings to `null` before persistence

---

## 👤 Author

Built as part of a legal clinic management platform for **Consultorios Jurídicos for PUCE**.  
Open to remote opportunities — feel free to connect!

---

## 📄 License

This project is for portfolio/demonstration purposes.
