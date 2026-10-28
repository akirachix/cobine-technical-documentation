---
hide:
  - navigation
---

# Technical Reference

This page brings together the application repositories and the code behind TerraWatch's core workflows. For a guided introduction, see the [Overview](overview.md); for local setup, see the [Developer Guide](developer-guide.md).

## Application Components

| Component | Role |
|---|---|
| Web application | Next.js and React workspace for analysts and administrators. |
| API | FastAPI services for rainfall, sectors, predictions, alerts, reviews, incidents, users, and audit records. |
| Operational data | PostgreSQL stores rainfall matrices and the records used across the analyst workflow. |
| Rainfall source | CHIRPS daily rainfall is retrieved through Google Earth Engine and summarized into sector-level time windows. |
| Risk assessment | Logistic Regression estimates landslide probability from rainfall and sector features. |

## Core Workflow

1. Rainfall synchronization retrieves CHIRPS observations and prepares rolling 1-, 3-, 7-, 10-, and 30-day values.
2. The prediction service combines the latest rainfall observation with the sector features used by the registered model.
3. TerraWatch stores the resulting probability, risk level, and feature snapshot.
4. Analysts review alerts, record decisions, and add incident reports as needed.

The [Data Pipelines](data-pipelines.md), [AI Risk Assessment](ai-risk-assessment.md), and [Analyst Workspace](analyst-workspace.md) guides describe each part in more detail.

## API Route Groups

| Route prefix | Resources |
|---|---|
| `/api/v1/` | Rainfall, sectors, alerts, reviews, and incidents. |
| `/auth/`, `/users/` | Authentication and user management. |
| `/analytics/` | Model versions, predictions, and feature snapshots. |
| `/audit-logs/` | Audit records. |

The model prediction route is `POST /analytics/models/{model_version_id}/predict-from-rainfall`. See the [prediction example](ai-risk-assessment.md#how-a-prediction-is-made) and [API reference](analyst-workspace.md#api-reference).

## Application Repositories

| Repository | Key areas |
|---|---|
| [Backend](https://github.com/akirachix/codebine-backend/tree/0fbb5a043f6d389fee865f19592bc1ade63e7372) | [Application routes](https://github.com/akirachix/codebine-backend/blob/0fbb5a043f6d389fee865f19592bc1ade63e7372/main.py), [rainfall service](https://github.com/akirachix/codebine-backend/blob/0fbb5a043f6d389fee865f19592bc1ade63e7372/services/rainfall_matrices.py), [inference service](https://github.com/akirachix/codebine-backend/blob/0fbb5a043f6d389fee865f19592bc1ade63e7372/services/risk_inference_service.py). |
| [Frontend](https://github.com/akirachix/cobine-frontend/tree/d3c2a10579c9a3d1deea7f79534b0245a85c50bb) | [API client](https://github.com/akirachix/cobine-frontend/blob/d3c2a10579c9a3d1deea7f79534b0245a85c50bb/src/lib/api.ts), [Next.js configuration](https://github.com/akirachix/cobine-frontend/blob/d3c2a10579c9a3d1deea7f79534b0245a85c50bb/next.config.ts). |

Use the [Developer Guide](developer-guide.md) to run the application and its checks.
