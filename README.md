# CareeriCS

**AI-assisted career preparation platform for Computer Science students and early-career developers.**

CareeriCS helps students move from academic learning to industry readiness through career exploration, structured learning roadmaps, skill assessment, CV workflows, interview preparation, and job discovery.

The platform is built as a full-stack application with a **Python/FastAPI backend**, **PostgreSQL/Supabase persistence**, and a **Next.js/React frontend**.

---

## Overview

Computer Science students often have access to large amounts of learning material but lack a structured path from university courses to real-world software roles.

CareeriCS brings the main parts of that journey into one platform:

- career-path exploration
- personalized learning roadmaps
- skill assessment and progress tracking
- CV building and enhancement
- AI-assisted interview preparation
- jobs and internship discovery
- course and learning-resource workflows
- user profiles and progress journeys
- reporting and career-related data

CareeriCS was developed as a team Computer Science graduation project.

---

## Architecture

```mermaid
flowchart LR
    A[Next.js / React Client]
    B[FastAPI REST API]
    C[Domain Routers]
    D[Service Layer]
    E[SQLAlchemy]
    F[(PostgreSQL / Supabase)]
    G[AI / Media Services]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    D --> G
```

The backend is organized around domain-specific routers and services rather than placing application logic directly inside API endpoints.

Major backend domains include:

- Career
- CV
- Interview
- Skills
- Skill Assessment
- Roadmaps
- Journey
- Jobs
- Courses
- Profiles
- Reports

---

## Tech Stack

| Area | Technologies |
| --- | --- |
| Backend | Python, FastAPI, Pydantic |
| Persistence | SQLAlchemy, PostgreSQL, Supabase |
| Frontend | Next.js, React, TypeScript |
| Styling | Tailwind CSS |
| AI / Media | OpenAI, Whisper, Transformers, DeepFace |
| HTTP / Integrations | HTTPX, Requests |
| Database Driver | psycopg2 |
| Observability | Request timing and SQL query instrumentation |
| Deployment | Railway-ready backend, Vercel-compatible frontend |

---

## Backend Highlights

### Domain-oriented API structure

FastAPI routers are separated by business domain and delegate application logic to service modules.

Example structure:

```text
routers/
├── career/
├── cv/
├── interview/
├── journey/
├── job/
├── roadmaps/
├── skill_assessment/
├── skills/
├── course.py
├── profile.py
└── reports/
```

This keeps HTTP concerns separate from application logic and makes the backend easier to extend and maintain.

### Database access

The backend uses **SQLAlchemy** with PostgreSQL-compatible connections.

Database configuration supports:

- `DATABASE_URL`
- `SUPABASE_DB_URL`
- individual PostgreSQL connection variables

The database engine uses connection health checks and pool recycling for more reliable long-running connections.

### Application startup validation

The FastAPI lifespan handler verifies database connectivity when the application starts and records the result through application logging.

### Observability

The backend includes instrumentation for:

- API request timing
- SQLAlchemy query observation
- query metrics
- external-call timing

This provides better visibility into backend performance instead of relying only on endpoint-level logging.

### Health endpoint

```http
GET /health
```

Example response:

```json
{
  "status": "ok"
}
```

### API / frontend integration

CORS configuration supports local development and configurable production origins.

The frontend can communicate with FastAPI directly or through the Next.js FastAPI proxy.

---

## Frontend

The frontend uses:

- Next.js 16
- React 19
- TypeScript
- Tailwind CSS
- Supabase client integration

The application consumes backend APIs for the platform's career, CV, interview, roadmap, job, course, and profile workflows.

---

## Project Structure

```text
CareeriCS/
├── AI_API/
│   └── fast-api-app/
│       ├── core/
│       ├── db/
│       ├── routers/
│       ├── services/
│       ├── tests/
│       ├── main.py
│       └── requirements.txt
│
├── NextJS-Frontend/
│   └── careerics/
│       ├── app/
│       ├── config/
│       ├── lib/
│       ├── public/
│       └── package.json
│
└── README.md
```

---

## Getting Started

### Prerequisites

You will need:

- Python 3
- Node.js
- npm
- PostgreSQL or a Supabase PostgreSQL database

---

### 1. Backend

Navigate to the FastAPI application:

```bash
cd AI_API/fast-api-app
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate the virtual environment and install the dependencies:

```bash
pip install -r requirements.txt
```

Create:

```text
AI_API/fast-api-app/.env
```

At minimum, configure a database connection:

```env
DATABASE_URL=postgresql://USER:PASSWORD@HOST:5432/DATABASE
```

Alternatively:

```env
SUPABASE_DB_URL=your_supabase_postgres_connection
```

Supabase-backed features can also use:

```env
SUPABASE_URL=your_supabase_url
SUPABASE_KEY=your_supabase_key
```

Additional AI, model, media, or deployment features may require their corresponding environment variables.

Start the backend:

```bash
uvicorn main:app --reload
```

The default local API is available at:

```text
http://localhost:8000
```

Health check:

```text
http://localhost:8000/health
```

FastAPI interactive documentation:

```text
http://localhost:8000/docs
```

---

### 2. Frontend

Navigate to the Next.js application:

```bash
cd NextJS-Frontend/careerics
```

Install dependencies:

```bash
npm install
```

Create:

```text
.env.local
```

Example local configuration:

```env
NEXT_PUBLIC_FASTAPI_URL=http://localhost:8000
FASTAPI_URL=http://localhost:8000

NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

Authentication or production deployments may require additional server-side variables such as:

```env
NEXTAUTH_SECRET=your_secret
NEXTAUTH_URL=http://localhost:3000
```

Start the frontend:

```bash
npm run dev
```

Open:

```text
http://localhost:3000
```

---

## Testing

The backend contains automated tests for infrastructure such as observability and request/query instrumentation.

Tests can be expanded as the platform evolves to cover service behavior and endpoint integration.

---

## Deployment

The repository contains Railway deployment guidance for the FastAPI backend.

The intended production architecture supports:

```text
Browser
   │
   ▼
Next.js / Vercel
   │
   ▼
FastAPI / Railway
   │
   ▼
PostgreSQL / Supabase
```

Interview media is stored locally by the backend, so deployments using that workflow should use persistent storage and avoid multiple replicas unless media storage is moved to shared/object storage.

---

## Contributors

CareeriCS was developed collaboratively by:

- Mariam Medhat
- Ahmed Walid
- Rita Andrew
- Muhammed Tareq
- Mohamed Aly

---

## License

This project is licensed under the **Apache License 2.0**.
