---
hide:
  - navigation
---

<style>
  .md-content .tw-lead-text { font-size: 1rem !important; }
  .md-content .tw-callout-panel { font-size: 0.9rem; border-left-color: #F7931E !important; color: #cbd5e1; background: rgba(255,255,255,0.03) !important; }
  .md-content .tw-callout-panel strong { color: #F7931E; }
  .md-content .tw-callout-panel a { color: #8EB69B; }
  .md-content .md-typeset h2 { color: #ffffff; font-size: 1.25rem; font-weight: 500; }
  .md-content .md-typeset h2::after { content: ""; display: block; width: 2.5rem; height: 3px; margin-top: 0.4rem; background: #F7931E; border-radius: 2px; }
  .md-content .md-typeset h3 { color: #F7931E; font-size: 0.95rem; }
  .md-content .md-typeset table:not([class]) { font-size: 0.72rem; }
  .md-content .tw-feature-card strong, .md-content .tw-step-body strong { font-size: 0.92rem; }
  .md-content .tw-feature-card span, .md-content .tw-step-body span { font-size: 0.82rem; }
</style>

# AI Risk Assessment

<p class="tw-lead-text">
  TerraWatch Rwanda uses a supervised machine learning model to estimate landslide risk at the sector level. The model combines rainfall history with static terrain and land cover characteristics to produce a probability-based risk score.
</p>

<div class="tw-callout-panel">
  Model outputs are <strong>risk estimates</strong>, not official warnings. All alerts require analyst review in the <a href="analyst-workspace.md">Analyst Workspace</a> before action.
</div>

---

## Model

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Algorithm</strong>
    <span>Logistic Regression (L2-regularized, C=1.0, class_weight='balanced', max_iter=2000).</span>
  </div>
  <div class="tw-feature-card">
    <strong>Pipeline</strong>
    <span>SimpleImputer(mean) → StandardScaler → OneHotEncoder(drop='first') → LogisticRegression.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Output</strong>
    <span>Probability of landslide occurrence (0-1), thresholded to risk levels.</span>
  </div>
</div>

---

## Features

| Feature | Type | Source | Description |
| :--- | :--- | :--- | :--- |
| `month` | Numeric | Derived from observation_time | Seasonality (1-12). |
| `rain_1day_mm` | Numeric | CHIRPS / incoming JSON | Daily rainfall. |
| `rain_3day_mm` | Numeric | CHIRPS / incoming JSON | 3-day accumulated rainfall. |
| `rain_7day_mm` | Numeric | CHIRPS / incoming JSON | 7-day accumulated rainfall. |
| `rain_10day_mm` | Numeric | CHIRPS / incoming JSON | 10-day accumulated rainfall. |
| `rain_30day_mm` | Numeric | CHIRPS / incoming JSON | 30-day accumulated rainfall. |
| `mean_slope_deg` | Numeric | Sector table (DEM-derived) | Mean terrain slope. |
| `land_cover_class` | Categorical | Sector table (LULC) | Dominant land cover (Cropland, Forest, Grass/Shrubland, Built-Up Area, Wetland, Waterbody). |

---

## Training Data

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Positives</strong>
    <span>442 landslide events (2016-2025) matched to CHIRPS rainfall.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Negatives</strong>
    <span>884 random sector-days without landslides (2:1 ratio).</span>
  </div>
  <div class="tw-feature-card">
    <strong>Split</strong>
    <span>Temporal — train ≤ 2021, test ≥ 2022.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Total</strong>
    <span>1,326 samples.</span>
  </div>
</div>

---

## Performance (Test Set)

| Metric | Value |
| :--- | :--- |
| AUC | 0.721 |
| F1-Score | 0.496 |
| Precision (landslide) | 0.60 |
| Recall (landslide) | 0.42 |
| Accuracy | 0.59 |

---

## Risk Level Mapping

| Score | Level |
| :--- | :--- |
| ≥ 0.70 | EXTREME |
| ≥ 0.50 | HIGH |
| ≥ 0.25 | MODERATE |
| < 0.25 | LOW |

---

## Inference Flow

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Request</strong>
      <span>Call <code>POST /analytics/inferences</code> with <code>model_version_id</code>, <code>sector_code</code>, and <code>rainfall_observations</code>.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Lookup</strong>
      <span>Backend looks up the sector by code, gets <code>land_cover_class</code> and <code>mean_slope_deg</code>.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Features</strong>
      <span>Build the 8-feature vector from rainfall observations and sector attributes.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Predict</strong>
      <span>Run through the pipeline to get <code>risk_score</code> (probability).</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">05</div>
    <div class="tw-step-body">
      <strong>Return</strong>
      <span>Return <code>risk_score</code> + mapped <code>risk_level</code>.</span>
    </div>
  </div>
</div>

```json
POST /analytics/inferences
{
  "model_version_id": "...",
  "sector_code": "...",
  "rainfall_observations": [{...}]
}
```

---

## Limitations

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Small sample size</strong>
    <span>442 positive events limit generalization.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Temporal shift</strong>
    <span>Test set has higher positive rate than training.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Spatial</strong>
    <span>No spatial autocorrelation modeling.</span>
  </div>
</div>
