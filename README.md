# TerraWatch Rwanda - Technical Documentation

**Landslide-risk intelligence platform for disaster-management decision support in Rwanda.**

TerraWatch Rwanda is a geospatial intelligence platform that continuously monitors environmental conditions across Rwanda to identify areas showing elevated landslide risk. Built for disaster-management analysts at MINEMA and district-level offices, the platform bridges the gap between fragmented environmental data sources and actionable, geographically specific decision-making.

This repository contains the documentation site. The application is maintained in the [backend](https://github.com/akirachix/codebine-backend) and [frontend](https://github.com/akirachix/cobine-frontend) repositories.

Start with the [platform overview](docs/overview.md), explore the [system architecture](docs/architecture.md), or jump to the [developer guide](docs/developer-guide.md).

---

## Key Capabilities

| Capability | Description |
|---|---|
| **Risk Assessment** | Logistic Regression estimates landslide probability and maps the score to a risk level for a sector. |
| **Rainfall Data** | CHIRPS daily rainfall is retrieved through Google Earth Engine and summarized into rolling sector-level windows. |
| **Analyst Workflow** | Review risk estimates, manage alerts, record decisions, and report incidents in the analyst workspace. |
| **Operational Records** | Rainfall, model versions, predictions, alerts, reviews, incidents, and related activity are stored for the workflow. |

---

## How It Works

1. **Collect** - Retrieve CHIRPS daily rainfall for sector geometries through Google Earth Engine and prepare rolling time windows.
2. **Assess** - Use rainfall and sector features to estimate a probability-based risk score and risk level.
3. **Alert** - Bring locations requiring attention into the analyst workflow.
4. **Review** - Analysts consider the estimate alongside current conditions, record observations, and decide whether further action is needed.
5. **Record** - Store predictions, reviews, and incidents to support operational follow-up.

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
│   ├── architecture.md       # System components, data flow, tech stack, security architecture
│   ├── data-pipelines.md     # Data ingestion, feature engineering, model training/inference
│   ├── ai-risk-assessment.md # ML model, risk levels, features, API reference
│   ├── analyst-workspace.md  # Workspace UI, alerts, reviews, incidents, API reference
│   ├── security-compliance.md # Accounts, roles, privacy, and secure deployment
│   ├── developer-guide.md   # Setup, run, test, and maintain the application repositories
│   ├── technical-reference.md # System components, application routes, source links
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

The build checks navigation and links, then writes the site to the git-ignored `site/` directory.

For application setup and checks, see the [Developer Guide](docs/developer-guide.md). The [Technical Reference](docs/technical-reference.md) links the main services and API route groups.

---

## Deployment

The **documentation site** is deployed to GitHub Pages via a GitHub Actions workflow (`.github/workflows/deploy.yml`). This is separate from application deployment.

- **Trigger:** On every push to `main`, or manually via `workflow_dispatch`
- **Process:** The workflow installs dependencies, builds the site in strict mode, uploads the artifact, and deploys to GitHub Pages
- **Environment:** `github-pages`

The backend repository includes a Heroku deployment workflow. Application release details are maintained in the [backend](https://github.com/akirachix/codebine-backend) and [frontend](https://github.com/akirachix/cobine-frontend) repositories.

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

> TerraWatch provides decision support. Official warnings and response decisions remain with the responsible authorities.
