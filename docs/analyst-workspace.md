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
  The workspace supports decisions; it does not make them. A model output is a <strong>risk estimate</strong>, not an official warning. Every alert waits for an analyst's decision, and every decision is recorded with the analyst's name and the time.
</div>

---

## Dashboard

[![TerraWatch analyst dashboard](assets/images/terrawatch-analyst-dashboard.png)](assets/images/terrawatch-analyst-dashboard.png){ target="_blank" title="Open the full-size image" }

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>📊 Summary Cards</strong>
    <span>The total number of incidents and alerts at a glance.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🗺️ Risk Map</strong>
    <span>Sectors coloured by risk level, with a legend filter, a search box and map, satellite and terrain views.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📍 Sector Overview</strong>
    <span>The selected sector's risk level, 7-day rainfall, average slope and past landslides.</span>
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
      <span>Open the risk map and check which sectors show High or Critical risk after the latest rainfall update.</span>
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
      <span>Record landslides reported from the field so the event history stays complete.</span>
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

Analysts log landslides reported from the field so TerraWatch keeps a complete record of real events.

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
      <span>The report appears at the top of the incidents table. The reporter and the time received are added by the system.</span>
    </div>
  </div>
</div>

---

## API Reference

All paths start with `/api/v1`. Every alert, review and incident is also written to the audit log; see [Security & Compliance](security-compliance.md).

| Method | Endpoint | Purpose |
| :--- | :--- | :--- |
| `GET` | `/sectors` | List sectors for the map and the district and sector pickers |
| `POST` | `/alerts` | Raise an alert from a HIGH or EXTREME prediction |
| `GET` | `/alerts` | List alerts, newest first, with an optional `alert_status` filter |
| `GET` | `/alerts/{alert_id}` | Get one alert |
| `POST` | `/reviews` | Record a decision on an alert or a prediction |
| `GET` | `/reviews` | List reviews, newest first |
| `POST` | `/incidents` | Log a field incident |
| `GET` | `/incidents` | List incident reports, newest first |
| `GET` | `/incidents/{report_id}` | Get one incident report |

| Code | Meaning |
| :--- | :--- |
| `201` | The alert, review or incident was created. |
| `401` | The user is not logged in, or the account is suspended. |
| `404` | The alert, prediction or report does not exist. |
| `409` | An alert already exists for the prediction, or the alert was already reviewed. |
| `422` | The request is invalid, for example a missing or unknown field, a future event date or an unknown sector. |