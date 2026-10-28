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

# System Architecture

<p class="tw-lead-text">
  TerraWatch Rwanda connects rainfall data, sector geography, risk assessment, and an analyst workspace to support landslide preparedness and response.
</p>

<div class="tw-callout-panel">
  The architecture brings environmental observations into a shared workflow: collect rainfall, estimate risk, and support analyst review.
</div>

---

## System Architecture Diagram

<div class="architecture-diagram">
  <a href="../assets/images/TerraWatch-System-Architecture-Diagram-by-Codebine.png" target="_blank" title="Open the full-size system architecture diagram">
    <img src="../assets/images/TerraWatch-System-Architecture-Diagram-by-Codebine.png" alt="TerraWatch Rwanda System Architecture Diagram" style="max-width:100%;height:auto;">
  </a>
</div>

*Figure 1. TerraWatch system design, showing the main components and information flows. The sections below describe the rainfall and prediction workflows.*

---

## System Components

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Data Layer</strong>
    <span>PostgreSQL keeps the sector, rainfall, model, prediction, alert, review, user, and incident records used by TerraWatch.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Data Pipelines</strong>
    <span>CHIRPS daily rainfall is retrieved through Google Earth Engine and summarized into rolling 1-, 3-, 7-, 10-, and 30-day sector values.</span>
  </div>
  <div class="tw-feature-card">
    <strong>AI Risk Assessment</strong>
    <span>Logistic Regression combines recent rainfall and sector characteristics to estimate landslide probability and map it to a risk tier.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Analyst Workspace</strong>
    <span>A Next.js and React workspace brings together the risk map, dashboard, alerts, incidents, analytics, and administration tools.</span>
  </div>
  <div class="tw-feature-card">
    <strong>API Layer</strong>
    <span>FastAPI provides the application endpoints for rainfall, predictions, alerts, reviews, incidents, users, and audit records.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Security & Compliance</strong>
    <span>Account sign-in, role-based permissions, request validation, and audit records support responsible use of the workspace.</span>
  </div>
</div>

---

## System Workflow

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Ingest Environmental Data</strong>
      <span>Sector characteristics and historical landslide records provide geographic and event context for analysis.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Prepare Risk Assessment</strong>
      <span>Historical landslide records are paired with environmental observations to train and evaluate the Logistic Regression model.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Ingest Incoming Rainfall</strong>
      <span>CHIRPS rainfall is retrieved through Google Earth Engine and the resulting sector-level observations are stored in PostgreSQL.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Generate Risk Estimates</strong>
      <span>The prediction service selects the latest timestamped observation, builds the estimator-specific feature row, records the inference and feature snapshot, and applies the configured score thresholds.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">05</div>
    <div class="tw-step-body">
      <strong>Alert & Review</strong>
      <span>HIGH and EXTREME inferences can be converted into alerts (HIGH and CRITICAL severities respectively). Reviews use APPROVED, REJECTED, or DISMISSED decisions; incidents are separate records.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">06</div>
    <div class="tw-step-body">
      <strong>Record & Audit</strong>
      <span>Alert, review, and incident activity is recorded to support traceability across the analyst workflow.</span>
    </div>
  </div>
</div>

---

## Technologies and Configuration

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Frontend</strong>
    <span>Next.js App Router, React, TypeScript, Leaflet/React-Leaflet, and Recharts. The frontend uses <code>NEXT_PUBLIC_API_BASE_URL</code> for backend routing.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Backend</strong>
    <span>Python 3.11+, FastAPI, Pydantic, SQLAlchemy, Alembic, PostgreSQL, and a joblib/scikit-learn inference artifact.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Relational Storage</strong>
    <span><strong>PostgreSQL</strong> — Stores operational records; the ORM models and Alembic migrations live in the backend repository.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Rainfall Source</strong>
    <span><strong>CHIRPS via Google Earth Engine</strong> — The integration reads daily rainfall by sector geometry. Required source credentials and dataset configuration are environment settings.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Deployment</strong>
    <span>The backend and documentation site have separate deployment workflows. See the [Developer Guide](developer-guide.md) for local setup and release references.</span>
  </div>
</div>

---

## Security Architecture

<div class="architecture-diagram">
  <a href="../assets/images/terrawatch-security-sad.png" target="_blank" title="Open the full-size security architecture diagram">
    <img src="../assets/images/terrawatch-security-sad.png" alt="TerraWatch Rwanda Security Architecture Diagram" style="max-width:100%;height:auto;">
  </a>
</div>

*Figure 2. Target security design showing proposed account, application, and data layers. See [Security & Compliance](security-compliance.md) for verified application controls and deployment responsibilities.*

Authentication, role permissions, and activity records support the analyst workflow. See [Security & Compliance](security-compliance.md) for account controls and deployment practices.

## Service Routes

The API groups operational resources under `/api/v1/`, authentication and users under `/auth/` and `/users/`, model and inference operations under `/analytics/`, and audit records under `/audit-logs/`. Browse the [hosted interactive API reference](https://terrawatch2-9d926d327adb.herokuapp.com/docs) for the deployed API. See the [selected workflow endpoints](analyst-workspace.md#selected-workflow-endpoints) and the [Technical Reference](technical-reference.md) for code links.

---

## Maintaining and Extending the System

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Understand Before Changing</strong>
      <span>Identify component responsibilities, inputs, outputs, and dependencies before making changes.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Assess Impact</strong>
      <span>Check whether changes affect connected components, configuration, data formats, or downstream outputs.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Test Thoroughly</strong>
      <span>Test the affected component and relevant end-to-end workflows using the project's actual development setup.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Update Documentation</strong>
      <span>Keep diagrams, configurations, and this documentation in sync with implementation changes.</span>
    </div>
  </div>
</div>
