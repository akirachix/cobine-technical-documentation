# TerraWatch Rwanda - Technical Documentation

**Landslide-risk intelligence platform for disaster-management decision support in Rwanda.**

TerraWatch Rwanda is a geospatial intelligence platform that continuously monitors environmental conditions across Rwanda to identify areas showing elevated landslide risk. Built for disaster-management analysts at MINEMA and district-level offices, the platform bridges the gap between fragmented environmental data sources and actionable, geographically specific decision-making.

---

## Key Capabilities

| Capability | Description |
|---|---|
| **Risk Map Visualization** | View landslide risk classified into Low, Moderate, High, and Critical zones across Rwanda by sector or district. |
| **ML Risk Assessment** | A Random Forest classifier evaluates environmental conditions and produces risk classifications with confidence scores. |
| **Alert Management** | Automated alerts are generated, organized, prioritized, and tracked through an auditable review workflow. |
| **Incident Logging** | Record confirmed landslide events with location, severity, timing, and supporting evidence for future learning. |
| **Data Quality Tracking** | Continuously assess dataset freshness, completeness, and geographic coverage. |
| **Audit Trail** | Maintain a complete, timestamped history of assessments, alerts, reviews, decisions, and incidents. |

---

## How It Works

1. **Collect** - Automatically retrieve rainfall observations, terrain/elevation data, and historical landslide records linked to geographic locations.
2. **Assess** - The Random Forest classifier evaluates each monitored area, producing a risk classification and an independent confidence score.
3. **Alert** - When risk thresholds are exceeded, a new alert is generated and queued for analyst review.
4. **Review** - Analysts review evidence, record observations, and determine whether further action is required.
5. **Learn** - The platform compares predictions against real-world incidents, identifies false alarms and missed events, and supports continuous model improvement.

---

## Project Structure

```
terrawatch-rwanda-docs/
├── .github/
│   └── workflows/
│       └── deploy.yml        # GitHub Actions CI/CD for GitHub Pages
├── docs/
│   ├── index.md              # Homepage (hero, cards, problem statement, scope)
│   ├── overview.md           # Platform overview, features, target users
│   ├── architecture.md       # Platform architecture (to be documented)
│   ├── data-pipelines.md     # Data ingestion and storage (to be documented)
│   ├── ai-risk-assessment.md # ML models and risk scoring (to be documented)
│   ├── analyst-workspace.md  # Workspace UI and workflows (to be documented)
│   ├── security-compliance.md # Security and compliance (to be documented)
│   ├── glossary.md           # Domain terms and acronyms
│   └── assets/
│       ├── images/           # Logos, diagrams, screenshots
│       └── stylesheets/
│           └── extra.css     # Custom CSS (brand colors, glassmorphism, responsive)
├── mkdocs.yml                # MkDocs configuration
├── requirements.txt          # Python dependencies
└── README.md                 # This file
```

---

## Running Locally

### Prerequisites

- Python 3.11+
- [MkDocs](https://www.mkdocs.org/) with the Material theme

### Setup

```bash
# Clone the repository
git clone https://github.com/akirachix/cobine-technical-documentation.git
cd cobine-technical-documentation

# Install dependencies
pip install -r requirements.txt

# Serve locally (auto-reload on changes)
mkdocs serve
```

The site will be available at `http://localhost:8000`.

### Build for Production

```bash
mkdocs build --strict
```

The built site is output to the `site/` directory (git-ignored).

---

## Deployment

This project is deployed to **GitHub Pages** via a GitHub Actions workflow (`.github/workflows/deploy.yml`).

- **Trigger:** On every push to `main`, or manually via `workflow_dispatch`
- **Process:** The workflow installs dependencies, builds the site in strict mode, uploads the artifact, and deploys to GitHub Pages
- **Environment:** `github-pages`

---

## Target Users

| User | Role | Engagement |
|---|---|---|
| **Disaster Management Analysts (MINEMA)** | Review and investigate risk assessments | Requires geographic dashboard and evidence visualization |
| **District Disaster Offices** | Local risk monitoring | Requires district-level visibility |
| **Meteo Rwanda** | Weather data provider | Provides rainfall information used by the system |
| **System Administrators** | Manage platform operations | Requires RBAC and monitoring |

---

## License

Internal technical documentation for the TerraWatch Rwanda project.
