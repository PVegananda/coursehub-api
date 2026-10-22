<div align="center">
  <img src="https://cdn.simpleicons.org/django/0C4B33" alt="Django logo" width="96" />
  <h1>CourseHub API</h1>
  <p>A clean, type-safe learning platform API built with Django Ninja.</p>

  <p>
    <a href="https://www.python.org/"><img src="https://img.shields.io/badge/Python-3.11-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.11"></a>
    <a href="https://www.djangoproject.com/"><img src="https://img.shields.io/badge/Django-5.1-092E20?style=for-the-badge&logo=django&logoColor=white" alt="Django 5.1"></a>
    <a href="https://django-ninja.dev/"><img src="https://img.shields.io/badge/Django%20Ninja-1.1-0C4B33?style=for-the-badge&logo=django&logoColor=white" alt="Django Ninja 1.1"></a>
    <a href="https://www.postgresql.org/"><img src="https://img.shields.io/badge/PostgreSQL-16-4169E1?style=for-the-badge&logo=postgresql&logoColor=white" alt="PostgreSQL 16"></a>
    <a href="https://www.docker.com/"><img src="https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker Compose"></a>
  </p>
</div>

## Overview

CourseHub API is a backend service for managing online courses and their content. It combines Django's mature web foundation with Django Ninja's typed schemas and automatic OpenAPI documentation to provide an API that is straightforward to consume and maintain.

The project includes authenticated course enrollment, role-aware CRUD operations, comments, search and price filtering, and a PostgreSQL-backed development environment.

## Highlights

- **Typed API contracts** with Pydantic schemas and Python type hints
- **Interactive API documentation** through Swagger UI and OpenAPI
- **JWT authentication** for registration, sign-in, token refresh, and protected resources
- **Course management** with search, price filters, enrollment, and personal course lists
- **Content and comment management** with ownership and role-based authorization
- **PostgreSQL persistence** with Django migrations and seed data
- **Containerized development** with Docker Compose
- **Query optimization** using `select_related` and `prefetch_related`

## Quick Start

### Prerequisites

- Docker Desktop with Docker Compose
- Git

### Run locally

```bash
git clone https://github.com/PVegananda/django-ninja.git
cd django-ninja

docker compose build
docker compose up -d
docker compose exec app python manage.py migrate
docker compose exec app python manage.py seed_data
```

The API is available at <http://localhost:8000>.

To stop the services:

```bash
docker compose down
```

To remove the local PostgreSQL volume as well:

```bash
docker compose down -v
```

> The default database credentials in `docker-compose.yml` are intended for local development only.

## API Documentation

Once the application is running, explore the API interactively:

- **Swagger UI:** <http://localhost:8000/api/v1/docs>
- **OpenAPI schema:** <http://localhost:8000/api/v1/openapi.json>
- **Health check:** <http://localhost:8000/api/v1/hello/>

### Main endpoints

| Area | Routes | Access |
| --- | --- | --- |
| Authentication | `POST /api/v1/register/`, `/api/v1/auth/*` | Public |
| Courses | `GET /api/v1/courses/`, `GET /api/v1/courses/{id}` | Public |
| Course management | `POST`, `PUT`, `DELETE /api/v1/courses/` | JWT required |
| Enrollment | `POST /api/v1/course/{id}/enroll/` | JWT required |
| My courses | `GET /api/v1/mycourses/` | JWT required |
| Content | `/api/v1/contents/` and `/api/v1/contents/{id}` | Read public; write protected |
| Comments | `/api/v1/comments/` and `/api/v1/comments/{id}` | JWT required |

Use the Swagger **Authorize** button with an access token to try protected endpoints.

## Example Requests

List courses:

```bash
curl http://localhost:8000/api/v1/courses/
```

Search and filter courses:

```bash
curl "http://localhost:8000/api/v1/courses/?search=python&min_price=50000&max_price=150000"
```

Create a course with an authenticated request:

```bash
curl -X POST http://localhost:8000/api/v1/courses/ \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <access-token>" \
  -d '{"name":"Practical Python","description":"Build useful Python applications.","price":99999}'
```

## Testing

Run the Django test suite inside the application container:

```bash
docker compose exec app python manage.py test
```

## Project Structure

```text
code/
├── courses/
│   ├── apiv1.py          # API routes and authorization rules
│   ├── schemas.py        # Request and response schemas
│   ├── models.py         # Course, content, enrollment, and comment models
│   ├── tests.py          # Application tests
│   ├── management/       # Management commands, including seed_data
│   └── migrations/       # Database migrations
├── lms/
│   ├── settings.py       # Django configuration
│   ├── urls.py           # Project URL configuration
│   ├── asgi.py
│   └── wsgi.py
├── manage.py
└── requirements.txt
Dockerfile
docker-compose.yml
```

## Technology Stack

- Python 3.11
- Django 5.1
- Django Ninja 1.1
- PostgreSQL 16
- Docker Compose
- Pydantic schemas
- JWT authentication

## Development Notes

This repository is configured for local development with `DEBUG=True`, a development PostgreSQL database, and sample course data. Before deploying to a hosted environment, replace the default credentials, configure secrets through environment variables, disable debug mode, and review the database and security settings.

## About

CourseHub API is a portfolio project demonstrating practical backend engineering with Django: designing typed REST endpoints, protecting resources with JWT authentication, modeling relationships, and documenting an API for a pleasant developer experience.
