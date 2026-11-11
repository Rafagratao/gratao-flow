# Gratão Flow

> 🇧🇷 [Leia em português](README.pt-BR.md)

Production management system built for **Gratão Uniformes**, a garment and uniform manufacturer in Brazil. It is in daily production use and tracks the full operation:

**Customer → Customer order → Service order → Production → Outsourcing → Delivery → Finance → Reports**

Version 2 is being re-implemented in [FlowSistem](https://github.com/Rafagratao/FlowSistem) based on what we learned running v1.

## Features

- Customers, customer orders and service orders
- Production stages: cutting, DTF printing and sewing
- Outsourced production with partner workshops
- Piece inventory by product and size
- Partial and full deliveries
- Payments, receivables and weekly financial closings
- Employees and time tracking (multiple shifts per day)
- PDF reports generated with ReportLab
- JWT authentication with role-based access

## Tech stack

| Layer | Technologies |
| --- | --- |
| Backend | Python 3.11, FastAPI, SQLAlchemy 2.0, Pydantic, Alembic, PostgreSQL, JWT, Passlib/bcrypt, ReportLab |
| Frontend | Next.js 14, React 18, TypeScript, Tailwind CSS, React Hook Form, Zod |
| Infrastructure | Docker, Docker Compose |

## Architecture

```
frontend/ (Next.js)  ──HTTP/JSON──▶  backend/ (FastAPI)  ──SQLAlchemy──▶  PostgreSQL
                                        │
                                        ├── api/routes     REST endpoints
                                        ├── services/      business rules
                                        ├── models/        ORM models
                                        ├── schemas/       Pydantic validation
                                        └── alembic/       database migrations
```

On startup the backend runs `alembic upgrade head`, applies a safe base seed (products, sizes, services and the admin user) and serves the API with Uvicorn.

## Running locally

Create a `.env` file from `.env.example` and set strong values:

```bash
ENVIRONMENT=development
SECRET_KEY=replace-with-a-strong-random-secret
ADMIN_EMAIL=admin@gratao.local
ADMIN_PASSWORD=replace-with-a-strong-admin-password
ADMIN_NAME=Administrador
POSTGRES_PASSWORD=replace-with-a-strong-postgres-password
CORS_ORIGINS=http://localhost:3000
```

Start everything:

```bash
docker compose -f docker/docker-compose.yml up --build -d
```

- Frontend: http://localhost:3000
- API docs: http://localhost:8000/docs

Log in with `ADMIN_EMAIL` and `ADMIN_PASSWORD`. To reset the local database to the base seed, run `./scripts/reset-database.ps1`.

## Production

Set `ENVIRONMENT=production`, a unique `SECRET_KEY`, a managed PostgreSQL `DATABASE_URL` and `CORS_ORIGINS` limited to the real frontend domain. In production `/docs`, `/redoc` and `/openapi.json` are disabled automatically.

## Author

Rafael Gratão · [LinkedIn](https://www.linkedin.com/in/rafael-gratao-96a150242) · [GitHub](https://github.com/Rafagratao)
