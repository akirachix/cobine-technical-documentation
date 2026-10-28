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
  .md-content .tw-feature-card strong, .md-content .tw-step-body strong { font-size: 0.92rem; }
  .md-content .tw-feature-card span, .md-content .tw-step-body span { font-size: 0.82rem; }
</style>

# Data Pipelines

<p class="tw-lead-text">
  Rainfall observations give TerraWatch a changing picture of conditions across Rwanda's sectors. The pipeline retrieves daily CHIRPS data, prepares rolling rainfall windows, and makes those observations available to the landslide-risk model.
</p>

<div class="tw-callout-panel">
  Rainfall synchronization and model training are separate workflows. New rainfall observations can support new risk estimates; they do not retrain the model.
</div>

---

## From Rainfall to Risk

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>🌧️ Rainfall Source</strong>
    <span>CHIRPS daily precipitation is retrieved through Google Earth Engine for sector areas.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📅 Rolling Windows</strong>
    <span>Daily values are summarized over 1, 3, 7, 10, and 30 days.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🗄️ Operational Data</strong>
    <span>Sector-level rainfall matrices and related records are stored in PostgreSQL.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🧠 Risk Assessment</strong>
    <span>The model uses rainfall history alongside the relevant sector features to estimate landslide probability.</span>
  </div>
</div>

---

## Rainfall Synchronization

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Select the Period</strong>
      <span>Request rainfall synchronization for a date range and, when needed, a specific sector.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Retrieve CHIRPS</strong>
      <span>Google Earth Engine returns daily rainfall observations for the selected sector areas.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Prepare the Series</strong>
      <span>The service calculates the rolling rainfall windows and associates them with sector and date.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Use in Assessment</strong>
      <span>Prediction requests combine the latest rainfall observation with the model's sector features.</span>
    </div>
  </div>
</div>

The API provides `POST /api/v1/rainfall/sync` for a requested date range and optional sector, with status and query endpoints for working with the resulting records.

---

## Model Features

| Feature | How it is used |
|---|---|
| Rainfall windows | 1-, 3-, 7-, 10-, and 30-day accumulated rainfall from the latest observation. |
| Month | Calendar month derived from the observation time. |
| Mean slope | Sector terrain characteristic, when included by the model. |
| Land cover | Sector land-cover category, when included by the model. |

The model's feature list defines which of these inputs it uses. See [AI Risk Assessment](ai-risk-assessment.md) for the feature table, request example, and risk-level mapping.

---

## Historical Data and Model Training

Historical landslide records are matched with rainfall observations to prepare examples for model training. The training workflow also samples sector-days without recorded landslides. TerraWatch uses Logistic Regression; its reported evaluation results and temporal test split are described in [AI Risk Assessment](ai-risk-assessment.md#model-evaluation).

Keep dataset dates, sector identifiers, rainfall units, and geographic definitions consistent when preparing new training data or updating the model. After a model update, confirm that the prediction inputs still match its feature list.

---

## When Rainfall Data Needs Attention

| What you notice | First things to check |
|---|---|
| Rainfall records are missing or outdated | The requested date range, Google Earth Engine access, and sync status. |
| Rainfall synchronization fails | Earth Engine credentials, CHIRPS settings, and the API's error response. |
| Predictions reject an input | Required rainfall values, observation time, and the registered model's feature list. |
| Sector rainfall looks inconsistent | Sector geometry, observation dates, and the rainfall windows associated with each date. |

For code-level references, see the backend [rainfall service](https://github.com/akirachix/codebine-backend/blob/0fbb5a043f6d389fee865f19592bc1ade63e7372/services/rainfall_matrices.py), [CHIRPS client](https://github.com/akirachix/codebine-backend/blob/0fbb5a043f6d389fee865f19592bc1ade63e7372/services/chirps_gee.py), and [rolling-window calculations](https://github.com/akirachix/codebine-backend/blob/0fbb5a043f6d389fee865f19592bc1ade63e7372/services/rainfall_math.py).
