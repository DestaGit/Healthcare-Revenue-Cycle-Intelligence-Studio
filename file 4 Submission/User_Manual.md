# RevenueCycle Intelligence Studio - User Manual

## Overview

RevenueCycle Intelligence Studio is a browser-based analytics
application for exploring synthetic or de-identified revenue cycle data.
All processing occurs locally in the browser.

## Getting Started

1.  Open `index.html` in a modern browser.
2.  The embedded synthetic dataset loads automatically on first use.
3.  Alternatively:
    -   Upload a CSV file.
    -   Load a built-in synthetic sample.
    -   Paste tabular data.

## Main Modules

-   Home: Executive overview.
-   Data Workspace: Import, validate, preview, and search data.
-   Revenue Leakage: Leakage metrics and trends.
-   Denial Intelligence: Denial analysis and Pareto chart.
-   KPI Center: Operational KPIs and configurable targets.
-   Executive Dashboard: Summary view.
-   Reports: Generate downloadable HTML reports.
-   Settings: Currency, precision, and local session preferences.

## Supported CSV Columns

RecordID, Month, Department, Payer, ClaimStatus, BilledAmount,
AllowedAmount, PaidAmount, DenialAmount, Underpayment, MissedCharge,
RecoveryAmount, DaysInAR, FirstPassAccepted, POSCollected, DenialReason,
AppealStatus

## Best Practices

-   Use synthetic or de-identified data.
-   Review validation messages before analysis.
-   Save sessions only on trusted devices.

## Troubleshooting

-   Empty dashboard: load data.
-   Upload error: verify CSV headers.
-   Missing charts: refresh browser.

## Privacy

No backend services are used. Data remains in the local browser unless
exported by the user.
