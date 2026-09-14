# Andromeda

A full-stack social networking platform with posts, chats, groups, pages, a marketplace, and a "Watch" video feed. Built as a modular Django backend with an Angular client, backed by both a relational and a graph database so the social graph and content data each live where they belong.

## Stack

**Frontend**
- Angular 17 (Material, CDK, CropperJS)
- Nginx reverse proxy in production

**Backend**
- Django 4.2 + Django REST Framework
- Django Channels + Daphne for WebSockets (chats, notifications)
- JWT auth (SimpleJWT)
- Celery for background tasks

**Data**
- PostgreSQL — relational data (users, posts, marketplace, pages, groups)
- Neo4j (via neomodel) — social graph (follows, friendships, recommendations)
- Redis — channels layer + cache
- RabbitMQ — Celery broker

**Ops**
- Docker Compose for local orchestration
- Grafana + Prometheus for monitoring

## Repository layout

```
client/       Angular 17 application
server/       Django project (andromeda) with per-domain apps:
              users, posts, chats, groups, pages,
              marketplace, notifications, watch
monitoring/   Grafana + Prometheus configuration
docker-compose.yml
dev.sh / dev.ps1   Local dev helper scripts (bash / PowerShell)
```

## Getting started

### Prerequisites
- Docker and Docker Compose
- Node.js 18+ and Python 3.11+ (only if you want to run client/server outside Docker)

### 1. Configure environment

```bash
cp .env.example .env
# Edit .env and set SECRET_KEY, database passwords, etc.
```

### 2. Bring up the stack

```bash
docker compose up --build
```

This starts Postgres, Neo4j, Redis, RabbitMQ, the Django backend, the Angular client, and the monitoring services.

### 3. Run migrations (first time only)

```bash
docker compose exec server python manage.py migrate
docker compose exec server python manage.py createsuperuser
```

### Default local URLs

| Service       | URL                      |
| ------------- | ------------------------ |
| Client        | http://localhost         |
| API           | http://localhost/api     |
| Django admin  | http://localhost/admin   |
| Neo4j browser | http://localhost:7474    |
| Grafana       | http://localhost:3000    |

## Development helpers

`dev.sh` (Linux/macOS) and `dev.ps1` (Windows) wrap common tasks — starting individual services, running tests, tailing logs. Run the script with no arguments to see the available commands.

## Testing

```bash
# Backend
docker compose exec server pytest

# Frontend
docker compose exec client npm run test:ci
```

## License

See repository for license details.
