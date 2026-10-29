# Fleet Retention Analytics

Why do customers cancel within the first 90 days despite positive onboarding scores,
and which signals predict that early churn?

## Data
Three exports (2026-10-22), not included in this repo:
- `accounts.csv` – CRM accounts (Salesforce)
- `subscriptions.csv` – subscription lifecycle (Chargebee)
- `product_events.csv` – product event log (telemetry pipeline)

## How to run
1. Put the three CSVs in a `data/` folder at the repo root
2. Install: `pip install pandas jupyter matplotlib`
3. Open notebooks in `notebooks/` in order

## Structure
- `notebooks/` – analysis notebooks, one per week
