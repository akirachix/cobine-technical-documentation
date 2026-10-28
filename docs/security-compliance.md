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
  TerraWatch supports institutional work with user accounts, risk estimates, and analyst decisions. Good security protects that work and the people who rely on it—from sign-in through data storage and deployment.
</p>

<div class="tw-callout-panel">
  Predictions support human decision-making. Analysts review alerts; official warnings and response decisions remain with the responsible authorities.
</div>

---

## Security at a Glance

<div class="tw-feature-microgrid">
  <div class="tw-feature-card">
    <strong>🔐 Account Access</strong>
    <span>Password hashing, email verification codes, and signed access tokens help protect user accounts.</span>
  </div>
  <div class="tw-feature-card">
    <strong>🛡️ User Roles</strong>
    <span>Administrator and analyst roles organize access to platform operations.</span>
  </div>
  <div class="tw-feature-card">
    <strong>✅ Request Validation</strong>
    <span>API schemas define the information each operation can accept.</span>
  </div>
  <div class="tw-feature-card">
    <strong>📜 Activity Records</strong>
    <span>Key alert, review, and incident actions are recorded to support traceability.</span>
  </div>
  <div class="tw-feature-card">
    <strong>👤 Human Review</strong>
    <span>Risk estimates enter an analyst workflow rather than making public decisions automatically.</span>
  </div>
</div>

---

## User Roles

| Role | Responsibilities |
|---|---|
| **System Administrator** | Manage user accounts and administrative operations. |
| **MINEMA Analyst** | Review risk estimates and alerts, record decisions, and report incidents. |

Users sign in with an institutional account; the platform does not provide public self-registration.

---

## Sign-in and Sessions

TerraWatch protects passwords with a dedicated password-hashing library. After password verification, sign-in uses a one-time email code before issuing signed JWT access and refresh tokens.

| Control | Behavior |
|---|---|
| Email verification code | Expires after five minutes. |
| Access token | RS256-signed JWT; default lifetime is five minutes. |
| Refresh token | RS256-signed JWT; default lifetime is seven days. |
| Login throttling | Repeated failed attempts trigger a temporary sign-in block. |
| Account status | Suspended accounts cannot use the shared authenticated-session flow. |

Keep signing keys, email credentials, and database settings in deployment configuration—not in the source code or browser-facing variables.

---

## Permissions and Activity

Administrator and analyst roles are checked by the operations that require them. Review decisions are associated with the signed-in analyst. Alert, review, and incident activity creates audit records that support follow-up and operational accountability.

The audit log is available through `/audit-logs/`. For a user's current permissions, refer to the specific operation and its role requirements in the API documentation.

---

## Protecting Data in Deployment

Production environments should protect connections and stored data, restrict access to secrets and databases, and maintain tested backups. Before a deployment is used operationally, agree on:

- HTTPS/TLS between users, the frontend, and the API.
- Encryption and access controls for databases, backups, and stored model files.
- Backup frequency, retention, recovery ownership, and recovery testing.
- User-data access, retention, and deletion procedures.
- Monitoring and response procedures for account or service incidents.

These are deployment responsibilities as well as application concerns. Set them for the environment where TerraWatch will operate and verify them as part of release readiness.

The security architecture diagram is a target design, not an inventory of controls verified in a deployed environment. The application code provides account authentication, password hashing, role checks on protected operations, request schemas, and audit records. Infrastructure protections such as a WAF/CDN, database encryption at rest, PostgreSQL row-level security, and tested backups must be verified separately for each deployment.

---

## Privacy and Compliance

TerraWatch user records support account access and administration. Limit access to authorized personnel and use account information only for approved institutional purposes.

Rwanda's Law No. 058/2021 relating to the protection of personal data and privacy should guide the handling of personal information. ISO/IEC 27001 can inform an information-security management programme. A project reference to a law or standard is not, by itself, a compliance assessment or certification; seek the appropriate institutional and legal review before operational deployment.

---

## Security Checks

Run the backend's automated checks when changing API or account behavior:

```bash
uv run --no-sync ruff check .
uv run --no-sync pytest --cov --cov-report=term-missing
```

For production releases, include security testing of authentication, role permissions, API inputs, dependencies, and deployment configuration. See the [Developer Guide](developer-guide.md) for running the application and its checks.
