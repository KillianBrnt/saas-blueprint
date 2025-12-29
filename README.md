# SaaS Blueprint

This repository contains the source code for a production-grade SaaS application, built with scalability and maintainability in mind.

## 🏗 Stack

- **Frontend**: Next.js (Pages Router), Tailwind CSS
- **Backend**: NestJS, Prisma
- **Database**: PostgreSQL
- **Cache**: Redis
- **Infra**: Docker, Docker Compose

## 🚀 Getting Started

### Prerequisites
- [Docker & Docker Compose](https://www.docker.com/)
- [Node.js](https://nodejs.org/) (optional, for local intellisense)

### Running the Project (Dev Mode)

This project is configured for **full hot-reloading** within Docker.

```bash
# Start all services (Frontend, Backend, DB, Redis)
docker compose up --build
```

- **Frontend**: [http://localhost:3000](http://localhost:3000) (Edits in `/frontend` reflect instantly)
- **Backend API**: [http://localhost:3001](http://localhost:3001) (Edits in `/backend` trigger recompile)
- **Database**: Port `5432`
- **Redis**: Port `6379`

### Stopping the Project

```bash
docker compose down
```

## 📂 Project Structure

- `frontend/`: Next.js application
- `backend/`: NestJS API
- `ARCHITECTURE.md`: Technical architecture details
- `docker-compose.yml`: Service orchestration

## 🛠 Troubleshooting

**Hot Reload not working on Windows?**
Ensure `WATCHPACK_POLLING=true` is set in `docker-compose.yml` (it is by default).

**Prisma Migrations**
To run migrations against the valid Docker database:
```bash
# Enter the backend container
docker exec -it saas-backend sh

# Run migration
npx prisma migrate dev
```
