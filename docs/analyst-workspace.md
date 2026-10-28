---
hide:
  - navigation
---

<style>
  .md-content .tw-lead-text { font-size: 1rem !important; }
  .md-content .tw-callout-panel { font-size: 0.9rem; border-left-color: #F7931E !important; }
  .md-content .tw-callout-panel strong { color: #F7931E; }
  .md-content .md-typeset h2 { color: #ffffff; font-size: 1.25rem; font-weight: 500; }
  .md-content .md-typeset h2::after { content: ""; display: block; width: 2.5rem; height: 3px; margin-top: 0.4rem; background: #F7931E; border-radius: 2px; }
  .md-content .md-typeset h3 { color: #F7931E; font-size: 0.95rem; }
  .md-content .md-typeset table:not([class]) { font-size: 0.72rem; }
  .md-content .tw-feature-card strong, .md-content .tw-step-body strong { font-size: 0.92rem; }
  .md-content .tw-feature-card span, .md-content .tw-step-body span { font-size: 0.82rem; }
</style>

# Analyst Workspace

<p class="tw-lead-text">
  The Analyst Workspace is where MINEMA analysts turn TerraWatch's risk estimates into decisions. In one place, an analyst can see which sectors are at risk, review the alerts raised by the model, record a decision and log landslides reported from the field.
</p>

<div class="tw-callout-panel">
  The workspace supports decisions; it does not make them. A model output is a <strong>risk estimate</strong>, not an official warning.
</div>

---

## Dashboard

[![TerraWatch analyst dashboard](assets/images/terrawatch-analyst-dashboard.png)](assets/images/terrawatch-analyst-dashboard.png){ target="_blank" title="Open the full-size image" }

The dashboard brings sector information, recent alerts, and incident reports together for an analyst's daily review.

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>📊 Summary Cards</strong>
    <span>The total number of incidents and alerts at a glance.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🗺️ Risk Map</strong>
    <span>Explore sectors with map navigation, search, and risk filters.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📍 Sector Overview</strong>
    <span>Review sector risk, rainfall, slope, and incident information.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🚨 Recent Alerts</strong>
    <span>The newest alerts with date, time, sector, district, severity and status.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📝 Recent Incidents</strong>
    <span>The newest field reports with date, sector, district and description.</span>
  </div>
</div>

---

## Daily Workflow

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Monitor</strong>
      <span>Open the risk map and review sectors with HIGH or EXTREME estimates.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Review</strong>
      <span>Open each alert waiting for review and check the sector's rainfall, slope and history.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Decide</strong>
      <span>Approve, reject or dismiss the alert and add a comment explaining why.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Log</strong>
      <span>Record landslides reported from the field for operational follow-up.</span>
    </div>
  </div>
</div>

---

## Alerts

An alert is raised when the model classifies a sector as **HIGH** or **EXTREME** risk. Low and moderate results never create an alert, and only one alert can exist for each prediction.

| Risk level | Alert severity |
| :--- | :--- |
| HIGH | HIGH |
| EXTREME | CRITICAL |

Every new alert starts as `PENDING_REVIEW` and changes only when an analyst records a decision.

| Status | Meaning |
| :--- | :--- |
| `PENDING_REVIEW` | Waiting for an analyst. |
| `APPROVED` | The analyst confirmed the risk and the alert should be acted on. |
| `REJECTED` | The analyst found the alert wrong, for example because of bad input data. |
| `DISMISSED` | The alert is valid but needs no action. |

---

## Risk Reviews

A review is the analyst's recorded decision on an alert or on a prediction.

| Rule | Detail |
| :--- | :--- |
| **Decision** | One of `APPROVED`, `REJECTED` or `DISMISSED`. |
| **Comment** | Optional, up to 2,000 characters. |
| **Reviewer and time** | Set by the system from the logged-in user; they cannot be typed in. |
| **Alerts** | Reviewed once only; the alert's status changes to the decision. |
| **Predictions** | Can be reviewed again, for example when new rainfall data arrives. |

---

## Incident Reports

Analysts can record field-reported landslides. These reports are operational incident records; a report is not necessarily independently verified ground truth.

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Open the form</strong>
      <span>On the Incidents page, select Report incident.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Choose the location</strong>
      <span>Select the District; the Sector list then shows only the sectors in that district.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Describe the event</strong>
      <span>Enter the event date, which cannot be in the future, and a description of what was observed.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Submit</strong>
      <span>The report appears in the incidents table with its event details and received timestamp.</span>
    </div>
  </div>
</div>

---

## Selected Workflow Endpoints

This is a selected workflow reference, not a complete API catalogue. The API groups routes by workflow: resource routes use `/api/v1`, model and inference routes use `/analytics`, and sign-in routes use `/auth`. Browse the [hosted interactive API reference](https://terrawatch2-9d926d327adb.herokuapp.com/docs) for the deployed API's operations and schemas. When developing locally, use `http://127.0.0.1:8000/docs`. See [Security & Compliance](security-compliance.md) for user roles and account access.

| Method | Endpoint | Purpose |
| :--- | :--- | :--- |
| `GET` | `/api/v1/sectors` | List sectors; supports province/district filters and pagination |
| `POST` | `/api/v1/alerts` | Create an alert from an `inference_id`; only HIGH/EXTREME predictions are accepted |
| `GET` | `/api/v1/alerts` | List alerts, optionally filtered by `alert_status`, with `limit` and `offset` |
| `GET` | `/api/v1/alerts/{alert_id}` | Get one alert |
| `POST` | `/api/v1/reviews` | Record a decision on an alert or inference; authenticated reviewer is recorded |
| `GET` | `/api/v1/reviews` | List reviews, optionally filtered by alert or inference |
| `POST` | `/api/v1/incidents` | Create a report using `sector_id`, `event_date`, and `description` |
| `GET` | `/api/v1/incidents` | List incident reports with `limit` and `offset` |
| `GET` | `/api/v1/incidents/{report_id}` | Get one incident report |
| `POST` | `/analytics/models/{model_version_id}/predict-from-rainfall` | Run inference from a sector and timestamped rainfall observations |
| `POST` | `/analytics/inferences` | Record an already-calculated inference; this endpoint does not itself run the model |
| `GET` | `/analytics/inferences/{inference_id}` | Retrieve a stored inference |
| `POST` | `/api/v1/rainfall/sync` | Sync CHIRPS daily data for an optional sector/date range |
| `GET` | `/api/v1/rainfall/status` | Read sync status; optional `include_chirps=true` queries the source |

| Code | Meaning |
| :--- | :--- |
| `201` | The alert, review or incident was created. |
| `400` | A request cannot be processed, such as a missing/invalid inference input or referenced sector. |
| `401` | Authentication is missing or invalid on a route that requires it. |
| `403` | A route-level role or account check denies the operation. |
| `404` | The alert, prediction or report does not exist. |
| `409` | An alert already exists for the prediction, or the alert was already reviewed. |
| `422` | Request validation failed, such as a future incident date or unsupported alert risk level. |

### Request and response notes

- `POST /api/v1/alerts` accepts only `{ "inference_id": "UUID" }`; severity, message, and pending-review status are derived by the backend.
- `POST /api/v1/reviews` accepts `alert_id` or `inference_id` (exactly one), a decision (`APPROVED`, `REJECTED`, or `DISMISSED`), and optional comments up to 2,000 characters.
- `POST /api/v1/incidents` requires a timezone-aware, non-future `event_date`, a known `sector_id`, and a non-empty description up to 2,000 characters.
- The inference request/response example and threshold mapping are documented in [AI Risk Assessment](ai-risk-assessment.md).
- For source links and route groups, see the [Technical Reference](technical-reference.md#application-repositories).