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

# Security & Compliance

<p class="tw-lead-text">
  TerraWatch Rwanda handles institutional accounts, landslide risk outputs and analyst decisions. A compromised account or a changed risk record could lead to a wrong decision during an emergency, so security is built into every layer of the platform: the dashboard, the API, the database and the hosting environment.
</p>

<div class="tw-callout-panel">
  This page follows the Codebine Security Requirements. Each control is marked <strong>In place</strong> when it already exists in the code, or <strong>Planned</strong> when it is agreed and being built.
</div>

---

## Security at a Glance

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>🔐 Authentication</strong>
    <span>Individual institutional accounts, Argon2id password hashing and multi-factor authentication for administrators.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🛡️ Authorization</strong>
    <span>Role-based access control on every request, with access denied by default.</span>
  </div>
  <div class="tw-feature-card">
    <strong>✅ Input Validation</strong>
    <span>Every request is checked on the server, and unknown fields are rejected.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🔒 Data Protection</strong>
    <span>HTTPS for all traffic, encryption at rest and secrets kept out of the code.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📜 Audit Trail</strong>
    <span>Every alert, review, incident and rainfall sync records who did what, and when.</span>
  </div>
  <div class="tw-feature-card">
    <strong>👤 Human Decisions</strong>
    <span>The model produces risk estimates; only an analyst can approve an alert.</span>
  </div>
</div>

---

## User Roles

TerraWatch is an institutional platform with no public or community accounts. An administrator onboards analysts; users cannot register themselves.

| Role | Responsibilities |
| :--- | :--- |
| **System Administrator** | Onboards analysts, suspends or reactivates accounts, and manages model and threshold settings. |
| **MINEMA Analyst** | Views the risk map, reviews alerts and predictions, and logs field incident reports. |

Each user is stored in the `users` table with a role (`ADMIN` or `ANALYST`) and an account status (`ACTIVE` or `SUSPENDED`).

---

## Security Controls

### Authentication and Sessions

| Control | Implementation | Status |
| :--- | :--- | :--- |
| Individual accounts | One account per person with email and password; no shared accounts. | In place |
| Suspended accounts | A suspended user is refused by the API. | In place |
| Password storage | Argon2id hashes only, with a minimum length of 12 characters. | Planned |
| Multi-factor authentication | Required for administrators and other privileged accounts. | Planned |
| Brute-force protection | Login attempts are rate-limited and the account is locked for 15 minutes after repeated failures. | Planned |
| Login token | Protected endpoints require a JWT signed with RS256; the token holds no sensitive data. | Planned |
| Session expiry | Sessions expire after inactivity, end when the browser closes, and are cancelled on logout. | Planned |

### Authorization

| Control | Implementation | Status |
| :--- | :--- | :--- |
| Server-owned fields | `reported_by`, `reviewed_by`, `reported_at` and `reviewed_at` are set by the server. A request that sends them is rejected. | In place |
| Role-based access control | Permissions are checked on every request in the FastAPI backend, and access is denied by default. | Planned |
| Object-level checks | Users can open only the records their role allows, backed by PostgreSQL Row-Level Security. | Planned |

### API and Data Protection

| Control | Implementation | Status |
| :--- | :--- | :--- |
| Input validation | Pydantic checks the type, format and range of every request; unknown fields return HTTP 422. | In place |
| Business rules | Rules such as "an event date cannot be in the future" are enforced on the server, not only in the browser. | In place |
| SQL injection protection | All database access uses SQLAlchemy with bound parameters. | In place |
| Data checks | Rainfall cannot be negative and duplicate readings are not stored. | In place |
| Secrets management | Database URLs and Earth Engine keys live in server environment variables and are never committed to Git. | In place |
| Encryption in transit | All traffic uses HTTPS with TLS. | In place |
| Generic errors | Users see short messages; keys, tokens, stack traces and database details are never returned. | In place |
| Rate limiting | Login attempts and API calls per user per hour are limited. | Planned |
| Encryption at rest | The PostgreSQL database and its backups are encrypted with AES-256. | Planned |
| Backups | Encrypted daily backups are kept for 30 days, with a copy at a second Rwanda-based site. | Planned |

---

## Audit Trail

Important actions are recorded in the `system_audit_logs` table so the team can always answer **who did what, and when**.

| Action | Audit entry |
| :--- | :--- |
| An alert is raised from a high-risk prediction | `EMERGENCY_ALERT_CREATED` |
| An analyst records a decision on an alert or prediction | `RISK_REVIEW_CREATED`, with the reviewer and the decision |
| An analyst logs a field incident | `INCIDENT_REPORT_CREATED` |
| Rainfall data is synced from CHIRPS | A rainfall sync entry |

<div class="tw-callout-panel">
  Each audit entry is saved in the <strong>same database transaction</strong> as the change it records. If the change fails, no entry is written, and a change can never be saved without its entry. Entries never contain passwords, tokens or keys.
</div>

---

## Human Decision Boundary

TerraWatch supports decisions; it does not make them. A high-risk prediction creates an alert with the status `PENDING_REVIEW`, and only an analyst can approve, reject or dismiss it. The decision is stored with the reviewer and the time, and the system cannot issue public warnings by itself.

---

## Privacy

| Principle | How TerraWatch applies it |
| :--- | :--- |
| **Purpose** | Personal data is used only for login, access control, administration, security monitoring and accountability. |
| **Minimization** | The `users` table holds only name, institutional email, role, account status and timestamps. |
| **Access** | Only system administrators can see user details. |
| **Sharing** | Personal data is not shared with third parties without an approved institutional or legal reason. |
| **Retention** | Account data is kept while the account is active and for 3 months after deactivation, then deleted or anonymized. |

---

## Compliance

| Standard | How it applies |
| :--- | :--- |
| **Rwanda Law No. 058/2021** on personal data and privacy | Collect only what is needed, use it only for its stated purpose, store it in Rwanda, encrypt it and follow the retention periods. A privacy review is done before deployment. |
| **ISO/IEC 27001** | Guides access control, encryption, audit logging, backups, supplier checks and testing. |
| **MINEMA and hosting policies** | Reviewed with the MINEMA or hosting cybersecurity team before deployment. |

---

## Security Testing

| Test | What it checks | Tools |
| :--- | :--- | :--- |
| Automated tests | Validation, rejected fields, server-owned fields, audit entries and database rules, on every pull request. | pytest |
| API security testing | Authentication, authorization, input validation, rate limiting and token handling against the OWASP API Security Top 10. | Postman, Burp Suite Community |
| Dependency scanning | Known vulnerabilities in Python and JavaScript libraries. | pip-audit, npm audit |
| Data-ingestion testing | Negative rainfall, wrong formats, future dates, duplicates and missing values. | pytest |