# RevenueCycle Intelligence Studio
## Architecture

## 1. Purpose

This document defines the technical architecture for RevenueCycle Intelligence Studio.

The product is a standalone, browser-based revenue cycle analytics application. Its core workflow runs entirely in the browser. Local files and embedded synthetic data are the default input methods. API Portal and MCP Portal are optional extension points and are not required for normal operation.

---

## 2. Architecture Principles

The application follows these principles:

- Single standalone `index.html`
- Client-side processing
- No required backend
- No required API key
- Synthetic and de-identified data only
- Modular analytics functions
- Shared data model across all modules
- Optional enterprise connectors
- Responsive and accessible interface
- Exportable results
- No silent external data transmission

---

## 3. High-Level Architecture

```text
User
  |
  v
Browser Application
  |
  +-- Input Layer
  |     +-- Embedded Synthetic Dataset
  |     +-- CSV Upload
  |     +-- Drag-and-Drop
  |     +-- Paste Table
  |     +-- Optional API Portal
  |     +-- Optional MCP Portal
  |
  +-- Validation and Normalization Layer
  |
  +-- Unified Revenue Cycle Data Model
  |
  +-- Analytics Engine
  |     +-- Revenue Leakage
  |     +-- Denial Intelligence
  |     +-- KPI Calculations
  |     +-- Trend Analysis
  |     +-- Opportunity Ranking
  |
  +-- Presentation Layer
  |     +-- Home
  |     +-- Data Workspace
  |     +-- Leakage Module
  |     +-- Denial Module
  |     +-- KPI Center
  |     +-- Executive Dashboard
  |     +-- Reports Center
  |     +-- Settings
  |
  +-- Output Layer
        +-- CSV Export
        +-- HTML Report
        +-- Print
        +-- Local Session Storage
```

---

## 4. Deployment Model

### 4.1 Standalone Deployment

The primary deployment artifact is:

```text
index.html
```

The file contains:

- HTML structure
- CSS styles
- JavaScript application logic
- synthetic dataset generator
- validation rules
- analytics calculations
- chart rendering
- report generation
- local storage controls

The application can be opened directly in a modern browser.

### 4.2 Hosted Deployment

The same file may be hosted on:

- GitHub Pages
- Netlify
- Cloudflare Pages
- an internal web server
- an approved enterprise portal

Hosting does not change the default client-side processing model.

---

## 5. Application Layers

## 5.1 Presentation Layer

The presentation layer provides the complete interface.

### Main views

- Home
- Data Workspace
- Revenue Leakage
- Denial Intelligence
- KPI Center
- Executive Dashboard
- Reports Center
- Settings

### Responsibilities

- navigation
- responsive layout
- input-source selection
- forms and controls
- data tables
- charts
- alerts and status messages
- report preview
- export actions
- accessibility support

The presentation layer renders results but does not own business calculations.

---

## 5.2 Input Layer

The input layer supports multiple data sources.

### Embedded Synthetic Dataset

Purpose:

- immediate product demonstration
- safe testing
- user training
- performance testing

Available sizes:

- 500 records
- 1,000 records
- 5,000 records

The dataset is fully synthetic and contains no real patient or billing data.

### CSV Upload

The user selects a local CSV file.

The application reads it with the browser `FileReader` API.

### Drag-and-Drop

The user drops a CSV file into the upload area.

The same parser and validation workflow is used.

### Paste Table

The user may paste:

- CSV text
- tab-separated values
- spreadsheet rows

The application detects the delimiter and converts the content into records.

### API Portal — Optional

The API Portal is a future enterprise connection point.

Configuration fields may include:

- API base URL
- resource path
- authentication method
- access token
- request timeout
- connection status

The standalone edition validates and saves configuration locally but does not require an API connection.

### MCP Portal — Optional

The MCP Portal is a future connection point for approved Model Context Protocol servers.

Configuration fields may include:

- MCP server endpoint
- transport type
- workspace
- access token
- connection status

The standalone edition keeps MCP optional and does not depend on it for analytics.

---

## 5.3 Validation and Normalization Layer

All input sources pass through the same validation workflow.

### Responsibilities

- verify required columns
- detect missing values
- identify duplicate record IDs
- convert numeric values
- normalize Boolean values
- standardize empty fields
- calculate a data quality score
- create validation messages

### Validation output

```text
ValidationResult
  - qualityScore
  - criticalIssues[]
  - warnings[]
  - information[]
```

Critical structural errors should be shown before analysis.

Warnings do not block the user unless the dataset cannot be interpreted.

---

## 5.4 Unified Data Model

Every input source is converted into the same internal data structure.

### Core fields

| Field | Type | Purpose |
|---|---:|---|
| RecordID | String | Unique record identifier |
| Month | String | Reporting period |
| Department | String | Operational department |
| Payer | String | Payer category |
| ClaimStatus | String | Paid or denied status |
| BilledAmount | Number | Submitted charge amount |
| AllowedAmount | Number | Contractual allowed amount |
| PaidAmount | Number | Payment received |
| DenialAmount | Number | Denied financial amount |
| Underpayment | Number | Contract variance |
| MissedCharge | Number | Uncaptured charge estimate |
| RecoveryAmount | Number | Recovered denied revenue |
| DaysInAR | Number | Account age |
| FirstPassAccepted | Boolean | First-pass claim result |
| POSCollected | Number | Point-of-service collection |
| DenialReason | String | Denial category |
| AppealStatus | String | Appeal outcome |

### Shared state

The application maintains:

```text
ApplicationState
  - source
  - rawData
  - filteredData
  - validation
  - filters
  - settings
  - KPI thresholds
  - generated report
```

All modules use the same state to prevent inconsistent results.

---

## 5.5 Analytics Engine

The analytics engine contains deterministic browser-side calculations.

### Revenue Leakage

```text
Total Leakage =
Denial Amount
+ Underpayment
+ Missed Charge
```

```text
Recovery Opportunity =
Total Leakage
- Recovery Amount
```

Outputs:

- total leakage
- leakage rate
- leakage by payer
- leakage by department
- monthly leakage trend
- leakage detail records

### Denial Intelligence

Outputs:

- denied claim count
- denial rate
- denied amount
- recovered amount
- appeal success rate
- denial reasons
- denial Pareto analysis
- monthly denial trend

### KPI Engine

Core KPIs include:

- Net Collection Rate
- Denial Rate
- Average Days in AR
- Clean Claim Rate
- Appeal Success Rate
- Leakage Rate

Example formulas:

```text
Net Collection Rate =
Paid Amount / Allowed Amount × 100
```

```text
Denial Rate =
Denied Claims / Total Claims × 100
```

```text
Clean Claim Rate =
First-Pass Accepted Claims / Total Claims × 100
```

```text
Leakage Rate =
Total Leakage / Billed Amount × 100
```

Each KPI receives a status:

- Green
- Amber
- Red
- Gray when data is unavailable

### Insight Engine

The insight engine creates descriptive findings from calculated results.

Examples:

- largest leakage source
- highest-risk payer
- leading denial reason
- department with the highest exposure
- largest recovery opportunity

The engine does not make clinical decisions or predictive claims.

---

## 5.6 Visualization Layer

The visualization layer uses browser canvas and HTML tables.

### Required visuals

- monthly financial trend
- monthly leakage trend
- leakage by payer
- denial Pareto chart
- denial trend
- KPI status distribution
- executive KPI summary
- detail tables

### Rendering rules

- charts must resize with the viewport
- values must derive from current filters
- empty states must be shown clearly
- chart labels must remain readable
- no external chart library is required

---

## 5.7 Filter Layer

Global filters may include:

- payer
- department
- month

Filter behavior:

1. Update shared filter state.
2. Rebuild the filtered dataset.
3. Recalculate all metrics.
4. Redraw all affected charts.
5. Refresh detail tables.
6. Refresh findings and KPI status.

The same filters apply consistently across analytics modules.

---

## 5.8 Reporting Layer

The reporting layer generates browser-based reports.

### Report types

- Executive Summary
- Revenue Leakage Report
- Denial Intelligence Report

### Report contents

- source information
- generation date
- key financial metrics
- KPI results
- findings
- methodology statement

### Output formats

Current:

- downloadable HTML
- printable browser view
- CSV detail exports

Future optional formats:

- PDF
- JSON
- Excel

---

## 5.9 Local Storage Layer

Local storage is optional.

### Stored settings

- currency
- percentage precision
- KPI thresholds
- API configuration without credentials
- MCP configuration without credentials
- optional saved dataset session

### Rules

- credentials must not be stored
- users can delete saved sessions
- saved data remains in the current browser
- local storage is not a substitute for enterprise security controls

---

## 6. Input Workflow

```text
Select Input Source
  |
  +-- Synthetic Sample
  +-- CSV File
  +-- Paste Table
  +-- API Configuration
  +-- MCP Configuration
  |
  v
Parse Input
  |
  v
Normalize Fields
  |
  v
Validate Dataset
  |
  +-- Critical Error -> Display and stop
  |
  +-- Valid or Warning -> Continue
  |
  v
Load Unified Data Model
  |
  v
Calculate Metrics
  |
  v
Render Dashboards and Reports
```

---

## 7. Optional Connector Architecture

## 7.1 API Portal Flow

Future implementation:

```text
User Configuration
  |
  v
Connection Validation
  |
  v
Authentication
  |
  v
API Request
  |
  v
Response Mapping
  |
  v
Unified Data Model
  |
  v
Standard Validation and Analytics
```

### Required controls

- explicit connection action
- configurable endpoint
- secure authentication handling
- timeout handling
- error messaging
- schema mapping
- response size controls
- audit logging in enterprise deployments

## 7.2 MCP Portal Flow

Future implementation:

```text
User Configuration
  |
  v
MCP Server Discovery
  |
  v
Capability Validation
  |
  v
Approved Resource Selection
  |
  v
Structured Data Retrieval
  |
  v
Schema Mapping
  |
  v
Unified Data Model
```

### Required controls

- approved MCP server list
- explicit user authorization
- capability restrictions
- resource selection
- schema validation
- credential protection
- connector audit records

Neither connector may bypass the shared validation layer.

---

## 8. Security and Privacy

### Default security model

- no server required
- no automatic network requests
- data processed in browser memory
- local files remain local
- synthetic data available for safe testing
- exports occur only through user action

### Data restrictions

The product should not be treated as a certified PHI processing system by default.

Users must use:

- synthetic data
- de-identified data
- approved operational data

### Enterprise connector requirements

Before enabling live API or MCP connections, an enterprise implementation must address:

- authentication
- authorization
- encrypted transport
- access logging
- token lifecycle
- data retention
- browser security headers
- content security policy
- organizational approval
- privacy review

---

## 9. Performance Architecture

### Target data volume

Recommended browser workload:

- 500 rows for demonstration
- 1,000 rows for normal analysis
- up to 5,000 rows for stress testing

### Performance controls

- aggregate data once per render cycle
- limit detail table rendering
- reuse normalized data
- avoid repeated parsing
- debounce search and resize events
- keep charts library-free
- minimize local storage writes

Large enterprise datasets should use a future API layer with server-side aggregation.

---

## 10. Reliability

The application must:

- show an empty state when no data is loaded
- reject unreadable files
- preserve core navigation after errors
- prevent invalid calculations
- avoid division-by-zero errors
- display unavailable KPI values as gray
- keep exports aligned with current filters
- avoid dead buttons
- avoid silent failures

---

## 11. Accessibility

The interface should support:

- semantic headings
- keyboard-accessible controls
- visible focus states
- readable contrast
- labeled forms
- descriptive button text
- responsive tables
- accessible modal dialogs
- chart summaries where practical

---

## 12. Module Boundaries

| Module | Responsibility |
|---|---|
| Navigation | View switching and responsive sidebar |
| Input Manager | File, sample, paste, API, and MCP inputs |
| Parser | CSV and tabular parsing |
| Validator | Required fields and quality checks |
| Normalizer | Data type conversion |
| State Manager | Shared application state |
| Filter Manager | Global filter application |
| Leakage Engine | Leakage calculations |
| Denial Engine | Denial calculations |
| KPI Engine | KPI formulas and thresholds |
| Insight Engine | Descriptive findings |
| Chart Renderer | Canvas visualization |
| Table Renderer | Data preview and detail tables |
| Report Engine | HTML report generation |
| Export Manager | CSV and HTML downloads |
| Storage Manager | Local settings and sessions |

---

## 13. Future Architecture Path

### Phase 1 — Current Standalone Product

- single `index.html`
- embedded synthetic data
- CSV input
- paste input
- local analytics
- reports and exports
- optional connector configuration UI

### Phase 2 — Controlled Connector Edition

- API integration
- MCP integration
- secure authentication
- schema mapping
- connector status monitoring

### Phase 3 — Enterprise Platform

- backend services
- centralized access control
- audit logging
- scheduled ingestion
- larger datasets
- enterprise identity
- governed report distribution

The standalone product must remain usable even if later enterprise editions are created.

---

## 14. Acceptance Criteria

The architecture is correctly implemented when:

- the application runs from one HTML file
- synthetic data can be loaded without external services
- CSV and pasted data use the same data model
- validation occurs before analytics
- all modules use shared filtered data
- KPI formulas are transparent
- reports use current calculations
- API and MCP inputs are visibly optional
- optional connector controls do not break local use
- credentials are not stored
- the interface works on desktop and mobile
- exports contain current filtered results
- no core button is inactive or misleading

---

## 15. Final Architecture Boundary

RevenueCycle Intelligence Studio is a focused revenue cycle analytics workspace.

Its core responsibility is to:

1. accept approved data,
2. validate and normalize it,
3. calculate revenue cycle metrics,
4. identify leakage and denial patterns,
5. present findings clearly,
6. export usable reports.

The standalone application is not intended to replace:

- an EHR
- a billing system
- a claims clearinghouse
- an enterprise data warehouse
- an identity platform
- a clinical decision system

API and MCP portals extend the input layer only. They do not change the core analytics workflow.
