---
hide:
  - navigation
---

# Developer Guide

TerraWatch brings together a [FastAPI backend](https://github.com/akirachix/codebine-backend), a [Next.js frontend](https://github.com/akirachix/cobine-frontend), and this documentation site. Use this guide to run the application locally and check your changes.

## What You Need

- Python 3.11 or newer and [`uv`](https://docs.astral.sh/uv/)
- Node.js and npm
- PostgreSQL for the backend
- Google Earth Engine access when working with CHIRPS rainfall synchronization

Keep local credentials in your environment or an untracked local configuration file.

## Run the Backend

From the backend repository:

```bash
uv venv
uv pip install -r pyproject.toml --group dev
```

Set the database and authentication settings for your local environment. To work with rainfall synchronization, configure the Google Earth Engine credentials and CHIRPS settings as well.

Create or update the database, then start the API:

```bash
uv run --no-sync python migrate.py
uv run --no-sync uvicorn main:app --reload
```

The API is available at `http://127.0.0.1:8000`. Check `GET /health` to confirm it is running; interactive API documentation is at `/docs`.

Run the backend checks before submitting a change:

```bash
uv run --no-sync ruff check .
uv run --no-sync ruff format --check .
uv run --no-sync pytest --cov --cov-report=term-missing
```

## Run the Frontend

From the frontend repository:

```bash
npm ci
```

Set `NEXT_PUBLIC_API_BASE_URL` to the backend origin, then start the development server:

```bash
npm run dev
```

Open `http://localhost:3000`. To check your changes:

```bash
npm run lint
npm run format:check
npm run build
```

## Connect the Application

The backend groups routes by feature:

| Route | Used for |
|---|---|
| `/api/v1/...` | Rainfall, sectors, alerts, reviews, and incidents |
| `/auth/...`, `/users/...` | Sign-in and user management |
| `/analytics/...` | Model versions, predictions, and feature snapshots |
| `/audit-logs/...` | Audit records |

The frontend forwards `/auth/...` and `/api/v1/...` through its Next.js rewrites. For endpoint details, see the [Analyst Workspace API reference](analyst-workspace.md#api-reference).

## Build the Documentation

From this repository:

```bash
pip install -r requirements.txt
mkdocs serve
```

Preview the site at `http://localhost:8000`. Before publishing, run:

```bash
mkdocs build --strict
```

## Deployment

The documentation site deploys to GitHub Pages. The backend has a Heroku deployment workflow. For application releases and environment settings, follow the instructions in the [backend](https://github.com/akirachix/codebine-backend) and [frontend](https://github.com/akirachix/cobine-frontend) repositories.

## Quick Checks

| If you see… | Try… |
|---|---|
| The API cannot connect to PostgreSQL | Check the local database is running and `DATABASE_URL` points to it; rerun migrations after schema changes. |
| The frontend cannot reach the API | Check `NEXT_PUBLIC_API_BASE_URL`, then restart the frontend server. |
| Rainfall synchronization fails | Check Google Earth Engine access and the CHIRPS configuration. |
| A prediction cannot find its model | Confirm a model version is registered and its artifact is available to the backend. |
