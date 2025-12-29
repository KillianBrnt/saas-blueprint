# SaaS Architecture

## 1. Purpose

This document defines the technical architecture of the SaaS in order to:
- Provide a single source of truth for the team.
- Support onboarding and knowledge sharing.
- Be consumed by automation and analysis tools (e.g. Antigravity).
- Ensure consistency, scalability, and maintainability.

---

## 2. High-Level Overview

The system follows a containerized, service-oriented architecture composed of:
- A web frontend.
- A backend API.
- A relational database.
- An in-memory cache.
- External services for authentication, billing, and email.

```mermaid
graph TD
    Client[Web Client] --> Frontend[Frontend (Next.js)]
    Frontend --> Backend[Backend API (NestJS - Docker)]
    Backend --> DB[(PostgreSQL)]
    Backend --> Cache[(Redis)]
    Backend --> Auth[Auth Service]
    Backend --> Stripe[Stripe]
    Backend --> Email[Email Service]
```

All internal services are deployed as **Docker containers**.

---

## 3. Frontend

### 3.1 Technologies
- **Framework:** Next.js
- **Language:** TypeScript
- **Styling:** Tailwind CSS
- **UI Components:** shadcn/ui, Radix UI
- **Data Fetching:** TanStack Query
- **Authentication Client:** Auth.js

### 3.2 Responsibilities
- UI rendering (SSR / CSR).
- Client-side routing.
- State management.
- API communication.
- Session handling.
- Route protection (authentication / authorization).

### 3.3 Frontend Structure (Directory Guidelines)

The frontend follows a **simplified layered structure** with **co-located unit tests**.

```text
/frontend
├── pages/                # Application routes and views
│   ├── index.tsx
│   ├── index.test.tsx
│   ├── login/
│   └── dashboard/
│
├── components/           # Reusable UI components
│   ├── Button.tsx
│   ├── Button.test.tsx
│   ├── Navbar.tsx
│   └── Navbar.test.tsx
│
├── services/             # API communication and logic
│   ├── auth.service.ts
│   ├── auth.service.test.ts
│   ├── user.service.ts
│   └── user.service.test.ts
│
├── hooks/                # Custom React hooks
│   ├── useAuth.ts
│   ├── useAuth.test.ts
│   └── useFetch.ts
│
└── types/                # Global TypeScript definitions
    └── index.ts
```

### 3.4 Frontend Rules
- No direct access to databases.
- No business-critical logic in UI components.
- Feature logic must live in `/features`.
- Shared components must remain stateless when possible.

---

## 4. Backend API

### 4.1 Technologies
- **Runtime:** Node.js
- **Framework:** NestJS
- **Language:** TypeScript
- **API Style:** REST
- **ORM:** Prisma

### 4.2 Responsibilities
- Business logic.
- Authentication and authorization.
- Data validation.
- External service integration.
- Webhook handling (Stripe, Auth, etc.).

### 4.3 Backend Structure (Directory Guidelines)

The backend follows a **layered architecture** focusing on separation of concerns by technical role with **co-located unit tests**.

```text
/backend
├── src/
│   ├── controllers/      # Request handlers
│   │   ├── auth.controller.ts
│   │   ├── auth.controller.spec.ts
│   │   └── user.controller.ts
│   │
│   ├── models/           # Data models and schemas
│   │   ├── user.model.ts
│   │   └── subscription.model.ts
│   │
│   ├── services/         # Business logic
│   │   ├── auth.service.ts
│   │   ├── auth.service.spec.ts
│   │   └── user.service.ts
│   │
│   ├── repositories/     # Database access (Prisma)
│   │   ├── user.repository.ts
│   │   ├── user.repository.spec.ts
│   │   └── subscription.repository.ts
│   │
│   ├── modules/          # Feature aggregation
│   │   ├── auth.module.ts
│   │   └── user.module.ts
│   │
│   ├── app.module.ts
│   └── main.ts
```

### 4.4 Backend Rules
- Controllers must be thin.
- Business logic lives in services.
- Database access only via repositories.
- Modules must be isolated and reusable.

---

## 5. Authentication & Authorization

### 5.1 Tooling
- **Auth.js**

### 5.2 Supported Methods
- OAuth (Google, GitHub, etc.).
- Credentials (email / password).

### 5.3 Sessions
- Secure cookie-based sessions.
- Database-backed or JWT-based sessions.
- CSRF protection enabled.

### 5.4 Authorization Model
- RBAC (Role-Based Access Control).
- Authorization enforced on the backend.

---

## 6. Database

### 6.1 Primary Database
- **PostgreSQL**

### 6.2 Role
- System of record (source of truth).
- Persistent storage for:
  - Users
  - Organizations
  - Subscriptions
  - Permissions
  - Domain data

### 6.3 Access Rules
- Accessed exclusively via Prisma.
- Versioned migrations.
- No raw SQL in production code.

---

## 7. Cache & Asynchronous Processing

### 7.1 Tooling
- **Redis**

### 7.2 Use Cases
- Caching frequently accessed data.
- Session storage (temporary).
- Rate limiting.
- Asynchronous jobs (emails, exports, webhooks).

### 7.3 Rules
- Redis is never a source of truth.
- Critical data must always exist in PostgreSQL.

---

## 8. Billing

### 8.1 Tooling
- **Stripe**

### 8.2 Features
- Subscriptions.
- Recurring billing.
- Backend-driven billing logic.

### 8.3 Security
- No payment logic on the frontend.
- Stripe webhooks used as the source of truth.

---

## 9. Emails & Notifications

### 9.1 Providers
- Resend or Postmark.

### 9.2 Use Cases
- Email verification.
- Password resets.
- System notifications.

---

## 10. Deployment & Infrastructure

### 10.1 Containerization
- All services are containerized using **Docker**.
- One container per service.
- Immutable images per release.

### 10.2 Orchestration
- Docker Compose (local development).
- Kubernetes or Docker Swarm (production).

### 10.3 Environments
- Development
- Staging
- Production

Each environment uses separate:
- Databases
- Redis instances
- Environment variables

---

## 11. CI / CD

- Docker image built per commit.
- Automated tests in CI.
- Image versioning and tagging.
- Automated deployments.

---

## 12. Observability & Security

- Structured logging.
- Error monitoring (Sentry).
- Secrets via environment variables.
- Network isolation between services.
- Principle of least privilege.

---

## 13. Core Principles

- **Type safety everywhere.**
- **Clear separation of concerns.**
- **Horizontal scalability.**
- **Simplicity before premature optimization.**
- **Security by default.**

---

## 14. Future Evolution

- GraphQL for selected domains.
- Event-driven architecture.
- Targeted microservices.
- Advanced feature flagging.

---
