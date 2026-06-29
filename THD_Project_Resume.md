# THD Dashboard — Project Resume

**Role:** Full-Stack Developer & Product Lead
**Stack:** React, Flask, Python, Prophet, BigQuery, JWT, WeasyPrint
**Deployment:** Render (production), Railway (prior)

---

## Project Summary

Built end-to-end analytics platform for KPI tracking, pre/post launch impact analysis, and automated reporting. Internal tool for feature launch measurement with counterfactual forecasting, anomaly detection, and stakeholder reporting.

---

## Technical Contributions

### Backend (Python / Flask)
- Designed REST API with 30+ endpoints covering auth, data ingestion, KPI summarization, anomaly detection, report generation, and prototype analytics ([backend/app.py](backend/app.py)).
- JWT auth via `flask_jwt_extended` with role-based routing (standard vs prototype users) ([backend/app.py:152](backend/app.py#L152)).
- Custom `NumpyEncoder` to serialize pandas/numpy types over JSON ([backend/app.py:48-60](backend/app.py#L48-L60)).
- Excel ingestion pipeline with pandas + openpyxl — parses `DailyData` + `FeatureConfig` sheets, coerces types, fills segment defaults ([backend/utils.py:7](backend/utils.py#L7)).
- Pre/post metric engine with statistical lift calculation and segment filtering.
- Anomaly detection module for KPI outlier flagging.
- Event tracker parser for launch-date annotations on timeseries.

### Forecasting & Analytics
- Prophet counterfactual modeling — fit on pre-period, forecast into post-period, compute lift vs baseline ([backend/prophet_model.py](backend/prophet_model.py)).
- Auto schema detection for uploaded data: infers date, numeric (KPI), and segment columns from arbitrary Excel files ([backend/prophet_model.py:22](backend/prophet_model.py#L22)).
- BigQuery integration via `google-cloud-bigquery` + `pandas-gbq` for warehouse data pulls.
- Multi-KPI Prophet runner for parallel forecast generation.

### Reporting & Email
- WeasyPrint-based PDF report generator with Jinja2 templating ([backend/pdf_generator.py](backend/pdf_generator.py)).
- Outlook/SMTP email service via Flask-Mail — send, draft, and current-user lookup ([backend/email_service.py](backend/email_service.py)).
- Report preview + send + draft endpoints for two pipelines (main dashboard + Reporting Hub).

### Frontend (React)
- SPA with React Router — protected routes, role-aware redirects, loading states ([frontend/src/App.js](frontend/src/App.js)).
- AuthContext provider for token lifecycle + verification ([frontend/src/context/AuthContext.js](frontend/src/context/AuthContext.js)).
- 25+ components covering Dashboard, MetricCard, MetricModal, KPIChart, FilterBar, SendReportModal, QueryInfoModal, AdvancedAnalysis, FeedbackButton.
- Prototype surface: ProtoDashboard, ProtoOrbChart, ProtoMetricCard, ProtoMetricModal, ProtoFilterBar — separate experimental UI track.
- Reporting Hub module: DataUploader, ProphetChart, ProphetModeler, LiftSummary, KPIOrbs, KPIDetailModal ([frontend/src/components/ReportingHub/](frontend/src/components/ReportingHub/)).
- HubSelector for multi-surface routing post-login.

### Infrastructure & DevOps
- Render deployment with keep-alive service at module level for Gunicorn compatibility ([backend/keep_alive.py](backend/keep_alive.py)).
- Build pipeline: `react-scripts` via node direct-call to bypass Linux permission issues on cloud builds.
- Relative API paths in production for single-origin deploy.
- Prior Railway + nixpacks configs with LD_LIBRARY_PATH for numpy runtime deps.
- `.dockerignore` + CORS middleware with dynamic origin + preflight handler.

### Feedback System
- In-app feedback button that posts to GitHub Issues API — converted from email-based flow to eliminate inbox noise ([frontend/src/components/FeedbackButton.js](frontend/src/components/FeedbackButton.js)).

---

## Soft Skills / Product

- **Scope decisions:** Split codebase into stable Dashboard + experimental Prototype + Reporting Hub tracks. Allowed parallel iteration without destabilizing production.
- **User feedback loop:** Shipped in-app feedback capture wired to GitHub Issues — closed loop from user report to tracked work item.
- **Deployment triage:** Diagnosed and fixed cascading deploy failures (numpy native deps, Gunicorn keep-alive scoping, react-scripts permissions, PORT var expansion) under live deadlines. Migrated Railway to Render mid-project when platform limits hit.
- **Stakeholder reporting:** Built PDF + email pipelines so non-technical stakeholders receive digestible impact reports without touching the app.
- **Incremental UX polish:** 10+ iteration commits on tooltip z-index, chart defaults, tag matching, reset buttons, label clarity — driven by direct user feedback.
- **Domain modeling:** Translated vague "measure launch impact" requirement into structured schema (DailyData + FeatureConfig) + counterfactual forecasting methodology.
- **Auth & access control:** Role-based routing added late in project without breaking existing users.

---

## Key Outcomes

- 42 commits across backend, frontend, ML, deployment, and UX.
- Production deployment live on Render.
- Two parallel analytics surfaces (Dashboard + Reporting Hub) serving different user workflows.
- Prophet-powered counterfactual lift measurement replacing naive pre/post averaging.
- GitHub-integrated feedback pipeline.
