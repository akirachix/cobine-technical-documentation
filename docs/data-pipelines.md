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

# Data Pipelines

<p class="tw-lead-text">
  TerraWatch Rwanda uses environmental data to understand conditions associated with historical landslides and support landslide-risk assessment. This page explains data sources, workflows, storage, quality checks, and maintenance considerations.
</p>

<div class="tw-callout-panel">
  The historical model-training workflow and incoming operational rainfall workflow are <strong>separate processes</strong>. Receiving new rainfall data does not automatically trigger model retraining.
</div>

---

## Pipeline Overview

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>🌧️ Rainfall Source</strong>
    <span><strong>CHIRPS</strong> — Climate Hazards Group InfraRed Precipitation with Station data provides incoming rainfall for risk assessment.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🗄️ Data Storage</strong>
    <span><strong>PostgreSQL</strong> for structured relational data; <strong>Amazon S3</strong> for incoming rainfall data files.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🏔️ Environmental Factors</strong>
    <span>Rainfall, slope, historical landslides, land cover, geology, and soil form the core feature set.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🧠 Model Development</strong>
    <span>Historical events and environmental conditions are used to train and evaluate the landslide-risk model.</span>
  </div>
</div>

---

## Data Workflow

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Historical Datasets</strong>
      <span>Collect historical landslide records and environmental datasets (slope, land cover, geology, soil) for model development.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Data Preparation</strong>
      <span>Align events to locations and time periods, engineer features, and prepare compatible datasets for training.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Model Training</strong>
      <span>Train and evaluate the landslide-risk model using prepared features. Feature definitions are preserved for consistency.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Incoming Rainfall</strong>
      <span>CHIRPS data is retrieved and stored in Amazon S3 according to the configured retrieval process.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">05</div>
    <div class="tw-step-body">
      <strong>Data Processing</strong>
      <span>Stored rainfall is validated, prepared, and made available to application components for risk assessment.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">06</div>
    <div class="tw-step-body">
      <strong>Risk Assessment</strong>
      <span>The trained model uses prepared inputs (including current rainfall) to generate sector-level risk estimates.</span>
    </div>
  </div>
</div>

---

## Six Environmental Factors

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>🌧️ Rainfall</strong>
    <span>CHIRPS provides incoming rainfall. For events, accumulated rainfall over preceding periods may be more relevant than daily totals alone.</span>
  </div>
  <div class="tw-feature-card">
    <strong>⛰️ Slope</strong>
    <span>Terrain steepness helps represent topographic susceptibility to slope failure.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📍 Historical Landslides</strong>
    <span>Recorded past events are used to learn patterns and associate them with environmental conditions.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🌿 Land Cover</strong>
    <span>Surface cover provides environmental context relevant to slope stability.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🪨 Geology</strong>
    <span>Geological conditions influence material strength and susceptibility to failure.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🌱 Soil</strong>
    <span>Soil characteristics affect infiltration, cohesion, and response to rainfall.</span>
  </div>
</div>

<div class="tw-callout-panel">
  <strong>Note:</strong> Exact rainfall windows, feature calculations, and training methods must match the implementation. Association does not imply a fixed universal triggering threshold.
</div>

---

## Incoming Rainfall: CHIRPS to Amazon S3

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Source</strong>
      <span>Retrieve CHIRPS rainfall data from the configured source.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Store</strong>
      <span>Store incoming data in Amazon S3 with a defined object structure and access controls.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Process</strong>
      <span>Validate, transform, and prepare rainfall data for use by the application according to the implemented workflow.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Assess</strong>
      <span>Prepared rainfall is supplied to the risk-assessment process using the model's configured inputs.</span>
    </div>
  </div>
</div>

**Implementation details to maintain:**

- How CHIRPS data is retrieved and how frequently
- File format and S3 object organization
- Validation and preparation steps
- Application access to stored data
- How rainfall is supplied to risk assessment

---

## Where Data Is Stored

| Store | Purpose | Notes |
| :--- | :--- | :--- |
| **PostgreSQL** | Relational storage | Sectors, alerts, reviews, incidents, users, audit logs. Maintain schema, migrations, and queries. |
| **Amazon S3** | Object storage | Incoming CHIRPS rainfall data. Maintain bucket/path, access, and retention. |
| **Heroku** | Application hosting | Hosts the app; S3 is an AWS service, not owned by Heroku. |

---

## Data Quality Checks

| Check | What to verify |
| :--- | :--- |
| **Completeness** | Required values and fields are present. |
| **Spatial alignment** | Datasets refer to compatible locations and coverage. |
| **Temporal alignment** | Rainfall periods correspond to intended events or assessment periods. |
| **Validity** | Values and formats meet processing requirements. |
| **Freshness** | Incoming rainfall data is appropriate for its intended use. |
| **Model compatibility** | Prepared inputs match the model's expected features. |

---

## Troubleshooting

<details>
<summary><strong>CHIRPS rainfall data is missing or outdated</strong></summary>

Check the configured retrieval process, observation period, retrieval errors, and whether expected data is present in Amazon S3.

</details>

<details>
<summary><strong>Rainfall data is stored but unavailable to the application</strong></summary>

Check the configured object location, application permissions, connection settings, and the code responsible for accessing the data.

</details>

<details>
<summary><strong>Environmental datasets do not align</strong></summary>

Review geographical coverage, coordinate reference systems, resolution, and procedures used to associate environmental conditions with historical events.

</details>

<details>
<summary><strong>Model training or risk assessment fails</strong></summary>

Check prepared feature structure, missing values, preprocessing steps, and compatibility between data and the model's expected inputs.

</details>

---

## Maintaining the Pipeline

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Document Sources</strong>
      <span>Record dataset sources and relevant versions as they change.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Preserve Definitions</strong>
      <span>Keep preparation steps and feature definitions used for training consistent and documented.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Validate Connections</strong>
      <span>Verify the relationship between historical events and environmental conditions when updating datasets.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Test & Update</strong>
      <span>Test changes to rainfall retrieval/storage, ensure model compatibility, and update this page when the workflow changes.</span>
    </div>
  </div>
</div>