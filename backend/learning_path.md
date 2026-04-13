# Backend — Learning Path

**MANDATORY: Complete modules in this exact order. Do not skip ahead.**

Each module must be fully completed and mastered before moving to the next. Earlier modules are prerequisites for later ones.

**Prerequisite**: Complete Fundamentals through at least module 16 (OOP). DSA progress can run in parallel.

---

## Module Order (Easiest → Hardest)

### Phase 1: Python Backend Foundations
*Core Python skills needed before touching any framework.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 1 | **Python Backend Core** | `python_backend_core/` | Fundamentals: Functions, OOP | Scripts vs modules, __name__ == "__main__", virtual environments, pip, project structure. The foundation for all backend work. |
| 2 | **File I/O** | `file_io/` | Python Backend Core | Reading/writing files, CSV, JSON, context managers (with statement). Every backend system reads and writes data. |
| 3 | **Exceptions** | `exceptions/` | File I/O | try/except/finally, custom exceptions, error hierarchies. Production code must handle failures gracefully. |
| 4 | **Modules / Packages** | `modules_packages/` | Exceptions | Imports, __init__.py, relative imports, package structure. Needed to organize any real project. |
| 5 | **OOP in Backend** | `oop_backend/` | Modules/Packages, Fundamentals: OOP | Design patterns, SOLID principles, dataclasses, abstract base classes. How OOP is actually used in production. |

### Phase 2: Web Framework & APIs
*Building actual HTTP services.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 6 | **FastAPI** | `fastapi/` | OOP in Backend | The modern Python web framework. Routes, request/response, Pydantic models, automatic docs. |
| 7 | **REST APIs** | `rest_apis/` | FastAPI | REST conventions, HTTP methods, status codes, request validation, response schemas. Design-level thinking. |
| 8 | **Request Lifecycle** | `request_lifecycle/` | REST APIs | Middleware, dependency injection, request context, error handling pipeline. How a request flows through your app. |
| 9 | **Auth** | `auth/` | Request Lifecycle | JWT, OAuth2, password hashing, session management, role-based access. Security is non-negotiable. |

### Phase 3: Data Layer
*Connecting your APIs to databases and caches.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 10 | **PostgreSQL** | `postgres/` | Auth, Fundamentals: Dicts | SQLAlchemy, psycopg2, connection pools, migrations, queries. The standard backend database. |
| 11 | **Redis** | `redis/` | PostgreSQL | Caching, sessions, rate limiting, pub/sub. The standard backend cache layer. |

### Phase 4: Advanced Backend
*Patterns that make production systems reliable and scalable.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 12 | **Async Python** | `async/` | Redis | asyncio, async/await, event loops, concurrent requests. Essential for high-throughput services. |
| 13 | **Background Jobs / Queues** | `queues_jobs/` | Async Python | Celery, task queues, job scheduling, retries. Offload heavy work from request handlers. |
| 14 | **Testing** | `testing/` | Queues/Jobs | pytest, fixtures, mocking, integration tests, API testing. Every module you've built needs tests. |

### Phase 5: Infrastructure & Deployment
*Getting your code running in production.*

| # | Module | Folder | Prerequisites | Why This Order |
|---|--------|--------|---------------|----------------|
| 15 | **Docker** | `docker/` | Testing | Dockerfiles, docker-compose, multi-stage builds, volumes. Containerize everything you've built. |
| 16 | **Observability** | `observability/` | Docker | Logging, metrics, tracing, health checks. Know what's happening in your running system. |
| 17 | **Deployment** | `deployment/` | Observability | CI/CD, cloud deployment, environment management, secrets. Ship your code. |
| 18 | **System Design** | `system_design/` | All above | Architecture patterns, scaling, caching strategies, message queues, load balancing. The capstone. |

---

## Gate Rules

Before advancing from module N to module N+1:

- [ ] All `practice.py` drills solved
- [ ] All `test_cases.py` passing
- [ ] Mini project completed (API route + integration)
- [ ] Can explain production tradeoffs from memory
- [ ] Agent has updated `mastery_score.md` and `progress.md`

## Current Position

**Prerequisite**: Complete Fundamentals first.
**Active Module**: Not started
