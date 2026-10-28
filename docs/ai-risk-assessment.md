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

# AI Risk Assessment

<p class="tw-lead-text">
  TerraWatch uses Logistic Regression to estimate landslide risk for a sector. It combines recent rainfall with relevant sector characteristics and returns a probability-based score that helps analysts decide where closer review may be needed.
</p>

<div class="tw-callout-panel">
  The score is a <strong>risk estimate</strong>, not a certainty or an official warning. Analysts review alerts and make decisions through the <a href="analyst-workspace.md">Analyst Workspace</a>.
</div>

---

## What Goes Into an Estimate

The prediction uses the latest rainfall observation provided for the sector. The model's input features include rainfall windows and, where required, sector characteristics.

| Feature | Meaning |
| :--- | :--- |
| `rain_1day_mm` | Rainfall accumulated over one day. |
| `rain_3day_mm` | Rainfall accumulated over three days. |
| `rain_7day_mm` | Rainfall accumulated over seven days. |
| `rain_10day_mm` | Rainfall accumulated over ten days. |
| `rain_30day_mm` | Rainfall accumulated over thirty days. |
| `month` | Calendar month of the observation. |
| `mean_slope_deg` | Mean slope recorded for the sector. |
| `land_cover_class` | Land-cover category recorded for the sector. |

Rainfall values come from the observation; month is derived from its timestamp. Sector characteristics are included when expected by the model.

---

## How a Prediction Is Made

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Request</strong>
      <span>Send a sector ID and its timestamped rainfall observations.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Prepare</strong>
      <span>TerraWatch selects the latest observation and adds the sector features used by the model.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Estimate</strong>
      <span>Logistic Regression returns a probability between 0 and 1.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Review</strong>
      <span>The API records the estimate and its feature snapshot for the analyst workflow.</span>
    </div>
  </div>
</div>

Use `POST /analytics/models/{model_version_id}/predict-from-rainfall`:

```json
{
  "sector_id": "UUID",
  "rainfall_observations": [
    {
      "observation_time": "2026-09-27T03:00:00+03:00",
      "rain_1day_mm": 12.5,
      "rain_3day_mm": 24.0,
      "rain_7day_mm": 40.2,
      "rain_10day_mm": 51.0,
      "rain_30day_mm": 101.3
    }
  ]
}
```

The response includes `inference_id`, `risk_score`, `risk_level`, `prediction_time`, and `feature_snapshot_id`. `POST /analytics/inferences` records a prediction that has already been calculated; use the model-version prediction route above to run an estimate.

---

## Risk Levels

| Probability score | Level |
| :--- | :--- |
| ≥ 0.70 | EXTREME |
| ≥ 0.50 | HIGH |
| ≥ 0.25 | MODERATE |
| < 0.25 | LOW |

The score represents the model's estimated probability of landslide occurrence. It is not a separate confidence measure.

---

## Model Evaluation

The project evaluation uses a temporal split, with training records before 2022 and test records from 2022 onward. The recorded training set contains 442 landslide samples and 884 sampled non-landslide records.

| Test-set measure | Recorded result |
| :--- | :--- |
| Accuracy | 60.07% |
| ROC AUC | 0.7168 |
| F1-score | 0.5255 |
| Precision (landslide) | 0.61 |
| Recall (landslide) | 0.46 |

These figures describe that evaluation run; they are not a guarantee of performance in every sector or season. Model results work best as one part of an analyst's assessment alongside current conditions and local knowledge.
