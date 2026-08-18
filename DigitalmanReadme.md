# REGMED-CONNECT RA Digital Twin
 
**Regulatory Affairs Digital Twin & Intelligence Platform**
 
> ⚠️ **Demo / Prototype Notice:** This is a Mini MVP prototype. All data is synthetic demo data. NOT REAL FDA DATA OR DECISION. Not validated for GxP use, regulatory submission, or autonomous regulatory decision-making.
 
---
 
## Project Overview
 
REGMED-CONNECT RA Digital Twin is a Regulatory Affairs Digital Twin & Intelligence Platform that enables RA teams to:
 
- **Know Your Submission** — Real-time tracking of submission status, lifecycle, deficiencies, and questions
- **Predict Regulatory Risk** — Deterministic rule-based risk scoring with full traceability
- **Understand Regulatory Change** — FDA Guidance, Federal Register, and ICH Guideline monitoring linked to affected submissions
- **Explain Risk with AI** — RA AI Copilot that explains risk, summarizes guidance, and identifies impact (never makes regulatory decisions)
 
---
 
## Business Problem
 
Traditional RA submission tracking is:
- **Reactive** — Teams discover problems after FDA raises deficiencies
- **Opaque** — Risk is assessed manually with no systematic traceability
- **Disconnected** — Regulatory changes are not automatically linked to affected submissions
- **Unexplainable** — Risk assessments cannot be traced to specific rules or evidence
 
REGMED-CONNECT RA Digital Twin solves this by creating a live digital representation of every submission with real-time risk scoring, regulatory intelligence, and explainable AI analysis.
 
---
 
## MVP Scope
 
### ✅ Included in Phase 1
 
| Feature | Description |
|---------|-------------|
| Submission Digital Twin | Live digital object for each submission with full lifecycle |
| Regulatory Event Twin | FDA Guidance, Federal Register, ICH Guideline tracking |
| Health Score | 0-100 composite score (Completeness + Timeliness + Review Quality) |
| Rule-based Risk Engine | Deterministic risk scoring with 10 rules |
| Risk Explainability | Every risk score traces to specific rules and evidence |
| Regulatory Intelligence | Regulatory events linked to affected submissions |
| Control Tower Dashboard | Portfolio-level KPIs and top risk submissions |
| RA AI Copilot | Explainable regulatory analysis (not regulatory decisions) |
| Evidence Traceability | Every answer links to rules, deficiencies, events, documents |
| Standalone index.html | Browser-only demo, no server required |
| FastAPI Backend | REST API with demo data |
| Automated Tests | 68 tests (Risk Engine + API) |
 
### ❌ Excluded from Phase 1
 
- Complete Digital Thread
- Enterprise QMS Integration
- CAPA / Deviation / Complaint Systems
- Automatic eCTD Generation
- FDA Review Simulator
- Autonomous AI Agent / Autonomous Regulatory Decision
- GxP Validated Production System
 
---
 
## Quick Start
 
### Option 1: Standalone Demo (No Server Required)
 
```bash
# Simply open in any modern browser:
open index.html
# or double-click index.html in your file explorer
```
 
### Option 2: Backend API
 
```bash
cd backend
pip install -r requirements.txt
uvicorn app.main:app --reload
 
# API: http://localhost:8000
# Docs: http://localhost:8000/docs
# Health: http://localhost:8000/health
```
 
### Option 3: Run Tests
 
```bash
cd backend
pip install -r requirements.txt
python -m pytest tests/ -v
# Expected: 68 passed
```
 
---
 
## Architecture
 
```
FDA / Regulatory Sources
        ↓
Data Ingestion Layer (Mock Feed in Demo Mode)
        ↓
Regulatory Event Twin
        ↓
┌─────────────────────────────────────────┐
│              Twin Core                  │
│  Submission Twin | Risk Engine          │
│  Regulatory Twin | Health Score         │
└─────────────────────────────────────────┘
        ↓
FastAPI REST API
        ↓
React / Next.js Frontend (Production)
Standalone index.html (Demo)
        ↓
Control Tower Dashboard + RA AI Copilot
```
 
See [docs/architecture.md](docs/architecture.md) for full architecture documentation.
 
---
 
## Technology Stack
 
| Layer | Technology |
|-------|-----------|
| Backend | Python 3.12, FastAPI, Pydantic |
| Database (Production) | PostgreSQL, Neo4j, OpenSearch |
| Frontend (Production) | React, Next.js, TypeScript, Recharts, React Flow |
| AI (Pluggable) | Azure OpenAI / OpenAI / Anthropic / Local Model |
| Demo Mode | JSON files, in-memory data, no external dependencies |
| Testing | pytest, FastAPI TestClient |
 
---
 
## Data Model
 
### Submission Object
 
```
Submission
├── id, product, authority, submission_type
├── submission_date, current_status
├── health_score (0-100), risk_score (0-100)
├── modules[], deficiency_count, open_questions
├── lifecycle[], current_lifecycle_step
├── Deficiency[] — type, severity, status
├── Question[] — source, status
├── Response[] — content, status
├── RegulatoryEvent[] — linked by affected_submissions
├── RiskScore — score, label, triggered_rules[]
└── HealthScore — completeness, timeliness, review_quality, penalties
```
 
### Submission Lifecycle
 
```
Draft → Submitted → Reviewing → Questioned → Responded → Negotiating → Approved
                                                                      ↘ Rejected
                                                                      ↘ Withdrawn
                                                                      ↘ On Hold
```
 
---
 
## Risk Engine
 
### Risk Score Formula
 
```
Risk Score = Σ(triggered rule impacts), capped at 100
 
Labels: 0-29 Low | 30-59 Medium | 60-79 High | 80-100 Critical
```
 
### Risk Rules (v1)
 
| Rule | Condition | Impact |
|------|-----------|--------|
| R-CMC-001 | deficiency_count > 2 | +25 |
| R-CRIT-001 | critical_deficiency == true | +20 |
| R-FDA-002 | fda_interaction_overdue == true | +15 |
| R-RESP-001 | response_delay_days > 30 | +15 |
| R-OVD-001 | submission_overdue == true | +15 |
| R-REG-001 | affected_by_high_impact_event | +15 |
| R-MOD-001 | missing_module == true | +10 |
| R-RESP-002 | response_delay_days 10-30 | +10 |
| R-HLTH-001 | health_score < 70 | +10 |
| R-OPEN-001 | open_questions >= 3 | +10 |
 
See [docs/risk-engine.md](docs/risk-engine.md) for full documentation.
 
---
 
## RA AI Copilot
 
### What AI Can Do
 
- ✅ Summarize regulatory documents
- ✅ Explain Risk Engine results
- ✅ Compare submissions
- ✅ Identify possible regulatory impact
- ✅ Generate suggested RA review questions
 
### What AI Cannot Do
 
- ❌ Determine if a submission will be approved
- ❌ Make autonomous regulatory decisions
- ❌ Override Rule Engine results
 
Every AI answer includes:
```
"AI-generated analysis — Regulatory decision must be made by qualified RA personnel."
```
 
See [docs/ai-governance.md](docs/ai-governance.md) for full AI governance documentation.
 
---
 
## Environment Variables
 
Copy `.env.example` to `.env` and configure:
 
```bash
cp .env.example .env
```
 
Key variables:
 
```env
DEMO_MODE=true                    # true = no external dependencies
AZURE_OPENAI_ENDPOINT=            # optional: for production AI
AZURE_OPENAI_API_KEY=             # optional: for production AI
DATABASE_URL=                     # optional: for production DB
```
 
See [.env.example](.env.example) for all variables.
 
---
 
## API Reference
 
| Endpoint | Description |
|----------|-------------|
| GET /health | Health check |
| GET /api/submissions | List all submissions |
| GET /api/submissions/{id} | Submission detail with risk explanation |
| GET /api/submissions/{id}/risk | Full risk explanation |
| GET /api/submissions/{id}/events | Regulatory events for submission |
| GET /api/regulatory-events | List all regulatory events |
| GET /api/risk-rules | List all risk rules |
| GET /api/control-tower | Control Tower KPIs |
| POST /api/ai/query | RA AI Copilot query |
 
See [docs/api.md](docs/api.md) for full API documentation.
 
---
 
## GitHub Deployment
 
See the [GitHub Configuration Guide](GITHUB_SETUP.md) for step-by-step instructions on:
- Creating the GitHub repository
- Configuring GitHub Pages (for index.html demo)
- Setting up GitHub Actions CI/CD
- Configuring branch protection rules
- Setting up repository secrets
 
---
 
## Docker Deployment
 
```bash
# Demo mode (backend only)
docker-compose up backend
 
# Full stack (when frontend is ready)
docker-compose up
 
# Production (uncomment postgres/neo4j/opensearch in docker-compose.yml)
docker-compose up
```
 
---
 
## Demo Instructions
 
See [demo/demo-scenario.md](demo/demo-scenario.md) for a complete 10-minute demo script.
 
**Quick Demo:**
1. Open `index.html` in browser
2. Click **Control Tower** → see portfolio risk
3. Click **510K-001** → see Digital Twin with risk explanation
4. Click **RA AI Copilot** → ask "Why is 510K-001 high risk?"
5. Click **Regulatory Intel** → see regulatory events linked to submissions
 
---
 
## Limitations
 
| Limitation | Notes |
|------------|-------|
| Demo data only | No real FDA data integration in Phase 1 |
| No authentication | Phase 2 feature |
| No real-time updates | Phase 2 feature |
| No GxP validation | Phase 3/4 feature |
| No eCTD integration | Phase 3 feature |
| LLM requires API key | Deterministic fallback available |
| No PostgreSQL/Neo4j | JSON files used in Demo Mode |
 
---
 
## Regulatory Disclaimer
 
This MVP is a prototype / demonstration system and is not validated for GxP use, regulatory submission, or autonomous regulatory decision-making.
 
Production deployment requires validated regulatory data ingestion, source verification, and appropriate regulatory governance.
 
AI-generated analysis — Regulatory decision must be made by qualified RA personnel.
 
---
 
## Roadmap
 
See [docs/roadmap.md](docs/roadmap.md) for full roadmap.
 
| Phase | Focus | Timeline |
|-------|-------|----------|
| Phase 1 (Current) | Mini MVP | ✅ Complete |
| Phase 2 | Digital Thread, Inspection Twin, Quality Twin | +8 weeks |
| Phase 3 | AI Review Twin, Predictive Risk, Global Intel | +16 weeks |
| Phase 4 | Enterprise Control Tower, GxP Validation | +24 weeks |
 
---
 
## Documentation
 
| Document | Description |
|----------|-------------|
| [docs/architecture.md](docs/architecture.md) | System architecture |
| [docs/api.md](docs/api.md) | API reference |
| [docs/risk-engine.md](docs/risk-engine.md) | Risk Engine documentation |
| [docs/ai-governance.md](docs/ai-governance.md) | AI governance framework |
| [docs/roadmap.md](docs/roadmap.md) | Product roadmap |
| [demo/demo-scenario.md](demo/demo-scenario.md) | Demo script |
| [GITHUB_SETUP.md](GITHUB_SETUP.md) | GitHub configuration guide |
 

