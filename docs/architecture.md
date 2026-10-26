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
  TerraWatch Rwanda is built as an integrated landslide-risk intelligence platform. It combines environmental data, machine learning, and analyst workflows to turn data into actionable risk insights for disaster-management decision support.
</p>

<div class="tw-callout-panel">
  Architecture documentation reflects the current system design. When components, dependencies, or security controls change, <strong>diagrams and sections must be updated</strong> to stay in sync with the implementation.
</div>

---

## System Architecture Diagram

<div class="architecture-diagram">
  <img src="../assets/images/TerraWatch-System-Architecture-Diagram-by-Codebine.png" alt="TerraWatch Rwanda System Architecture Diagram" style="max-width:100%;height:auto;">
</div>

*Figure 1. TerraWatch Rwanda system architecture overview showing component relationships and data flows.*

---

## System Components

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Data Layer</strong>
    <span>Environmental datasets, historical landslide records, and incoming rainfall data stored across PostgreSQL and Amazon S3.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Data Pipelines</strong>
    <span>CHIRPS rainfall ingestion, data preparation, feature engineering, and alignment of environmental factors for model training and inference.</span>
  </div>
  <div class="tw-feature-card">
    <strong>AI Risk Assessment</strong>
    <span>Trained landslide-risk model that processes prepared inputs to generate sector-level risk estimates with confidence indicators.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Analyst Workspace</strong>
    <span>Web interface for reviewing alerts, recording decisions, logging field incidents, and visualizing risk on interactive maps.</span>
  </div>
  <div class="tw-feature-card">
    <strong>API Layer</strong>
    <span>RESTful endpoints under <code>/api/v1</code> that connect the frontend, model outputs, and persistent storage with audit logging.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Security & Compliance</strong>
    <span>Authentication, authorization, audit trails, and security controls that enforce accountability across all system actions.</span>
  </div>
</div>

---

## System Workflow

<div class="tw-step-grid">
  <div class="tw-step-node">
    <div class="tw-step-badge">01</div>
    <div class="tw-step-body">
      <strong>Ingest Environmental Data</strong>
      <span>Historical datasets (rainfall, slope, land cover, geology, soil, historical landslides) are collected and prepared for model development.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">02</div>
    <div class="tw-step-body">
      <strong>Train & Evaluate Model</strong>
      <span>Features are engineered from aligned historical events and conditions. The model learns patterns associated with landslides and is evaluated before deployment.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">03</div>
    <div class="tw-step-body">
      <strong>Ingest Incoming Rainfall</strong>
      <span>CHIRPS rainfall data is retrieved on the configured schedule and stored in Amazon S3 for processing and use in risk assessment.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">04</div>
    <div class="tw-step-body">
      <strong>Generate Risk Estimates</strong>
      <span>Prepared inputs are passed to the trained model to produce sector-level risk classifications (LOW/MODERATE/HIGH/EXTREME) for the current conditions.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">05</div>
    <div class="tw-step-body">
      <strong>Alert & Review</strong>
      <span>HIGH or EXTREME predictions trigger alerts in the Analyst Workspace. Analysts review, decide (APPROVE/REJECT/DISMISS), and log field incidents as needed.</span>
    </div>
  </div>
  <div class="tw-step-node">
    <div class="tw-step-badge">06</div>
    <div class="tw-step-body">
      <strong>Record & Audit</strong>
      <span>All actions are persisted to PostgreSQL and written to the audit log, maintaining a complete record for accountability and traceability.</span>
    </div>
  </div>
</div>

---

## Technologies and Configuration

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Relational Storage</strong>
    <span><strong>PostgreSQL</strong> — Stores sectors, alerts, reviews, incidents, users, and audit records. Schema and migrations live in the application codebase.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Object Storage</strong>
    <span><strong>Amazon S3</strong> — Stores incoming CHIRPS rainfall data. Access is configured via environment variables and application credentials.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Rainfall Source</strong>
    <span><strong>CHIRPS</strong> — Climate Hazards Group InfraRed Precipitation with Station data, used as the incoming rainfall feed.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Deployment</strong>
    <span><strong>Heroku</strong> — Application hosting environment. Note: Heroku hosts the app; Amazon S3 remains an AWS service.</span>
  </div>
</div>

---

## Security Architecture

<div class="architecture-diagram">
  <img src="../assets/images/terrawatch-security-sad.png" alt="TerraWatch Rwanda Security Architecture Diagram" style="max-width:100%;height:auto;">
</div>

*Figure 2. TerraWatch Rwanda security architecture showing security boundaries and controls.*

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>Authentication & Access</strong>
    <span>Role-based access ensures only authorized analysts can review alerts, make decisions, and log incidents.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Auditability</strong>
    <span>Every alert, review, and incident is written to the audit log with user identity and timestamp.</span>
  </div>
  <div class="tw-feature-card">
    <strong>Secure Boundaries</strong>
    <span>Data flows are constrained by service boundaries with controlled access between components and external services.</span>
  </div>
</div>

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
