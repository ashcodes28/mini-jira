# mini-jira

A minimal, production structured Scrum board built from first principles to master relational data modeling, state transitions, and full stack API design.

Most task trackers hide system complexity behind heavy abstractions. This project strips the problem down to its core primitives: authenticated users, a single board, hierarchical work items, and deterministic status transitions.

## Scope (v1)

- **Authentication:** Stateless JWT registration and login protecting all board endpoints.
- **Single Scrum Board:** Three fixed workflow columns (`TODO`, `IN_PROGRESS`, `DONE`) with drag and drop state updates.
- **Hierarchical Work Items:** Unified data model supporting `EPIC`, `STORY`, `TASK`, `BUG`, and `SUB_TASK` via self referential parent relationships.
- **Item Metadata:** Title, description, type, status, priority (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`), and assignee.

### Explicit Non Goals (Deferred to v2)
- Role based access control (RBAC) and multi tenant workspaces
- Automated sprint velocity analytics and burndown charts
- Dependency graph visualization engines

## Architecture and Data Model

```text
minijira/
├── backend/            # FastAPI, SQLAlchemy, Pydantic, JWT auth, state rules
├── frontend/           # React, TypeScript, Tailwind CSS, @dnd-kit
├── docker-compose.yml  # Local PostgreSQL and API orchestration
└── .env.example        # Environment variable template
```

### Design Decisions
- **Single Table Work Item Hierarchy:** All issue types (`EPIC` through `SUB_TASK`) live in a single `work_items` table with a nullable `parent_id` foreign key referencing `work_items.id`. Hierarchy constraints, such as preventing a `SUB_TASK` from acting as a parent, are enforced at the service layer rather than across fragmented tables.
- **Backend Enforced State Transitions:** Drag and drop actions on the client dispatch explicit status updates validated by the API, ensuring the database remains the single source of truth.

## Quickstart

### Prerequisites
- Docker and Docker Compose
- Python 3.11+ and Node.js 20+ (if running services outside containers)

### 1. Configure Environment
```bash
git clone https://github.com/ashcodes28/minijira.git
cd minijira
cp .env.example .env
```

### 2. Start Services
```bash
docker compose up --build
```

- **API and Interactive Docs:** `http://localhost:8000/docs`
- **Frontend Board:** `http://localhost:5173`

## Engineering Conventions

This repository follows [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`) and short lived feature branches merged into `main`.

## License

MIT
