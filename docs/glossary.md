---
hide:
  - navigation
  - toc
---

# Glossary

A reference for acronyms and domain terms used throughout the TerraWatch Rwanda documentation.

---

## Acronyms

| Acronym | Full Form | Description |
|---|---|---|
| **API** | Application Programming Interface | RESTful endpoints under `/api/v1` that connect the frontend, model outputs, and persistent storage. |
| **CHIRPS** | Climate Hazards Group InfraRed Precipitation with Station | Near real-time rainfall dataset used as the primary precipitation feed. |
| **GIS** | Geographic Information System | Systems and tools used for capturing, storing, checking, and displaying spatial data. |
| **HTTPS** | HyperText Transfer Protocol Secure | The secure version of HTTP, used for all traffic between clients and the platform. |
| **MINEMA** | Ministry in Charge of Emergency Management (Rwanda) | The institution identified in the project materials as the primary disaster-management stakeholder. |
| **OWASP** | Open Worldwide Application Security Project | A nonprofit foundation that produces security checklists and tools; the API security testing follows the OWASP API Security Top 10. |
| **REST / RESTful** | Representational State Transfer | An architectural style for designing networked applications; the platform exposes RESTful API endpoints. |
| **RBAC** | Role-Based Access Control | A permission model where access rights are assigned to users based on their role within the organization. |
| **S3** | Simple Storage Service | Amazon's object storage service used for storing incoming CHIRPS rainfall data and other assets. |
| **TLS** | Transport Layer Security | A cryptographic protocol used to secure communications over a network; all HTTPS traffic uses TLS. |

---

## Domain Terms

| Term | Definition |
|---|---|
| **Alert** | A notification generated when a sector's risk exceeds a predefined threshold (HIGH or EXTREME). Alerts appear in the Analyst Workspace review queue. |
| **Analyst** | A disaster-management analyst, typically from MINEMA, who reviews risk estimates, investigates alerts, makes decisions, and logs field incidents. |
| **Confidence** | A measure of certainty or evidence quality. TerraWatch's probability score represents estimated landslide risk, not a separate confidence measure. |
| **Critical** | Alert severity assigned by the backend to an `EXTREME` risk inference. It is not a fifth risk-level tier. |
| **District** | An administrative division in Rwanda; districts contain multiple sectors. |
| **Geospatial** | Relating to or denoting geographical information that has embedded spatial coordinates. |
| **High Risk** | A risk level indicating significant landslide likelihood requiring analyst attention. |
| **Incident report** | A field-reported landslide event recorded with its location, severity, timing, and supporting details. |
| **Low Risk** | The lowest risk level, indicating minimal landslide likelihood. |
| **Moderate Risk** | A risk level above low but below high, indicating moderate landslide likelihood. |
| **Model** | A serialized estimator registered through a model-version record. The backend inference service calls `predict_proba`; the exact estimator type and training lineage must be confirmed for each artifact. |
| **Risk Assessment** | The process of calculating a probability-based `risk_score` and mapping it to LOW, MODERATE, HIGH, or EXTREME for a sector. |
| **Sector** | The smallest administrative division in Rwanda used by TerraWatch for landslide risk monitoring. |
| **Trace** | To trace the origin and decision path of a risk estimate; the audit trail maintains a complete record for accountability. |
