---
hide:
  - navigation
---

# Overview

<p class="tw-lead-text">
  TerraWatch Rwanda helps disaster-management teams bring rainfall information and landslide-risk estimates into one sector-focused workspace. Analysts can review risk, manage alerts, and record incidents to support timely, informed decisions.
</p>

<div class="tw-callout-panel">
  TerraWatch is a decision-support tool: its risk estimates inform analyst review and do not replace official warnings or decisions.
</div>

---

## How It Works

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Collect</strong>
      <span>Bring sector-level rainfall observations and relevant geographic information together for review.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Assess</strong>
      <span>A Logistic Regression model estimates landslide probability from recent rainfall and sector features, then maps the result to a risk level.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Alert</strong>
      <span>Analysts create and prioritize alerts for locations that need attention.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Review</strong>
      <span>The analyst reviews the evidence, records observations, and determines whether further action is required.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">05</div>
    <div class="tw-step-body">
      <strong>Learn</strong>
      <span>Prediction, review, and incident records give teams a shared history of their work.</span>
    </div>
  </div>
</div>

---

## Key Features

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>🗺️ Geographic Risk Map</strong>
    <span>Explore sector-level rainfall information and landslide-risk estimates.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🤖 ML Risk Assessment</strong>
    <span>View a probability-based estimate and its LOW, MODERATE, HIGH, or EXTREME risk level.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🚨 Alert Management</strong>
    <span>Organize alerts, record review decisions, and keep attention focused on emerging risk.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📝 Incident Logging</strong>
    <span>Record landslide incidents with location, severity, timing, and supporting details.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📊 Data Quality Tracking</strong>
    <span>Follow rainfall synchronization and dataset records used by the platform.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📈 Analytics Dashboard</strong>
    <span>Bring together risk, alert, and incident information for operational review.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🔒 Audit Trail</strong>
    <span>Keep a record of key workflow actions and decisions.</span>
  </div>
</div>

---

## Target Users

| User | Role |
| :--- | :--- |
| **MINEMA analysts** | Review risk estimates, alerts, and incident information. |
| **District disaster-management teams** | Use sector-level information to support local coordination. |
| **System administrators** | Manage platform users and operations. |

---

## Decision Support

TerraWatch brings environmental information and risk estimates into an analyst workflow. Official warnings and response decisions remain with the responsible authorities.

For the [system architecture](architecture.md), [risk assessment](ai-risk-assessment.md), [analyst workflow](analyst-workspace.md), and [developer setup](developer-guide.md), continue to the relevant guide.
