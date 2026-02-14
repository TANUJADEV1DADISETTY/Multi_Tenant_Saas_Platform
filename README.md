# Multi-Tenant SaaS Platform

## Project Overview

This project is a **production-ready Multi-Tenant SaaS Platform** designed to allow multiple organizations (tenants) to independently manage their users, projects, and tasks with strict data isolation.

Each tenant operates independently, while a system-level **Super Admin** has global access and management capabilities.

The platform demonstrates:

- Multi-tenancy with strict isolation
- Role-Based Access Control (RBAC)
- Secure JWT authentication
- Containerized deployment
- Automated database initialization
- Production-ready backend architecture

---

## Core Features

- Multi-tenant architecture with tenant-level data isolation
- Role-based access control (`super_admin`, `tenant_admin`, `user`)
- Secure JWT-based authentication & authorization
- Tenant registration with automatic admin provisioning
- Tenant-scoped user management
- Project management with status tracking
- Task management with assignment, priority, and status
- Dashboard with real-time statistics
- PostgreSQL database with seeded demo data
- Fully containerized using Docker & Docker Compose
- Automated database initialization at startup
- Health check endpoint for service readiness

---

## Technology Stack

### Frontend

- React (Vite)
- React Router
- Axios
- Custom CSS

### Backend

- Node.js
- Express.js
- JWT (jsonwebtoken)
- bcrypt
- express-validator

### Database

- PostgreSQL 15

### DevOps & Tooling

- Docker
- Docker Compose
- Nginx (optional for production frontend)
- Git

---

## Architecture Overview

The platform follows a **three-tier architecture**:

### 1. Frontend (React)

- Handles UI and user interactions
- Communicates with backend via REST APIs
- Stores JWT token for authentication

### 2. Backend API (Node.js + Express)

- Manages authentication and authorization
- Enforces tenant isolation
- Provides RESTful APIs for users, projects, and tasks
- Validates role-based access control

### 3. Database (PostgreSQL)

- Stores tenants, users, projects, and tasks
- Maintains relationships using foreign keys
- Ensures referential integrity

---

## Multi-Tenancy Strategy

- Each user belongs to a specific tenant (except `super_admin`)
- `tenant_id` is embedded inside JWT token
- All database queries are filtered using `tenant_id`
- Super admin has `tenant_id = NULL` for global visibility
- Strict isolation enforced at query level

---

## Installation & Setup

### Prerequisites

- Docker
- Docker Compose
- Git

---

### Clone Repository

```bash
git clone https://github.com/manasa1349/multi-tenant-saas.git
cd multi-tenant-saas
```

---

### Environment Variables

Environment variables are configured via Docker Compose.

#### Backend Variables

| Variable       | Description                            |
| -------------- | -------------------------------------- |
| DB_HOST        | Database host                          |
| DB_PORT        | Database port                          |
| DB_NAME        | Database name                          |
| DB_USER        | Database username                      |
| DB_PASSWORD    | Database password                      |
| JWT_SECRET     | JWT signing secret (min 32 characters) |
| JWT_EXPIRES_IN | JWT expiry duration                    |
| FRONTEND_URL   | Allowed CORS origin                    |
| NODE_ENV       | Application environment                |

---

## Start Application (MANDATORY METHOD)

```bash
docker-compose up -d
```

This single command will:

- Start PostgreSQL database
- Initialize schema and seed data automatically
- Start backend API service
- Start frontend application

No manual migrations or seed commands are required.

---

## Service Verification

### Backend Health Check

```bash
curl http://localhost:5000/api/health
```

Expected response:

```json
{
  "status": "ok",
  "database": "connected"
}
```

### Frontend Access

Open in browser:

```
http://localhost:3000
```

---

## Database Initialization

- Database schema and seed data are auto-loaded
- PostgreSQL init scripts are mounted in `database/init`
- Initialization runs automatically when container starts
- Evaluation requires only:

```bash
docker-compose up -d
```

---

## Seeded Test Credentials

### Super Admin

- Email: `superadmin@system.com`
- Password: `Admin@123`
- Role: `super_admin`

### Tenant Admin

- Tenant Subdomain: `demo`
- Email: `admin@demo.com`
- Password: `Demo@123`
- Role: `tenant_admin`

### Regular User

- Tenant Subdomain: `demo`
- Email: `user1@demo.com`
- Password: `User@123`
- Role: `user`

---

## Application Routes

### Public Routes

- `/register` – Tenant registration
- `/login` – User login

### Protected Routes

- `/dashboard` – Overview dashboard
- `/projects` – Projects list
- `/projects/:projectId` – Project details & tasks
- `/users` – Tenant user management (tenant_admin only)

---

## API Documentation

Complete API documentation available at:

```
docs/API.md
```

Includes:

- All 19 endpoints
- Request/response schemas
- Authentication requirements
- Example payloads

---

## Health Check Endpoint

```
GET /api/health
```

Returns success only after:

- Database connection established
- Seed data loaded
- Backend ready to accept requests

---

## Docker Configuration Summary

- Single command startup
- Fixed ports:
  - Database: `5432`
  - Backend: `5000`
  - Frontend: `3000`

- Service-based networking
- Backend depends on database health
- Frontend depends on backend health
- Fully containerized architecture

---

## Limitations

- UI is functional but minimal
- No email notifications implemented
- No pagination for large datasets
- Basic task assignment UI
- No real-time updates (WebSockets not implemented)

---

## License

This project is licensed under the **MIT License**.

You are free to use, modify, and distribute this software for educational or commercial purposes.

---

## Conclusion

This project demonstrates a complete, production-ready SaaS platform with:

- Multi-tenancy
- Secure authentication & authorization
- Role-based access control
- Containerized deployment
- Automated database initialization
- DevOps readiness

It follows backend security best practices and scalable SaaS architecture principles, making it suitable for real-world SaaS systems.
