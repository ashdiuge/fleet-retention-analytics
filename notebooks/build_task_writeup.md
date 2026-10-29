**What I built:** `notebooks/01_schema_and_profile.ipynb` loads all three exports, checks keys and joins, maps every field, profiles the retention-related columns and measures data quality issues before any churn number is calculated. It also writes `schema_map.csv` (field-level map with annotations) and `week1_issues.csv` (issue log).

**Access and integrity:** All three exports load. Primary keys are unique and every subscription and event joins to an account. 21 accounts have no events at all.
- accounts: 1,014 rows · subscriptions: 1,055 · product_events: 56,999 (2025-04-30 to 2026-10-21, five app releases)

**Key summary stats:**
- Onboarding satisfaction: median 8.55, range 7.12–9.82. No low scores at all on a 1–10 scale, so on its own it cannot separate customers.
- Median ACV $98k; fleet_size median 347 (95 missing).
- Subscriptions: 671 active, 360 cancelled, 24 paused; 161 pilots; 41 accounts have two subscriptions.
- route_optimization_run (the core value action) is the most frequent event but reaches the fewest accounts (639 of ~990 with events), which makes it a promising signal for Week 3.

**What has to be cleaned or excluded first:**
1. 14 internal/test accounts (IDs AC901xxx), ~3.6× more active than real accounts. Exclude.
2. `contract_tier` has 12 spellings of 3 values (casing, trailing spaces). Normalise with strip/lower.
3. 10 ACV values are ~100× the rest; divided by 100 they fall in the normal range, so they look like cents. They nearly double the mean ACV ($184k → $99k).
4. 281 duplicate events: all `session_started`, all from app 4.3.0 on iOS/Android. Sessions per active day are 2.0 on that release vs ~1.0 everywhere else, so it looks like a client double-logging bug.
5. 36 events across 32 accounts are timestamped before the account's subscription started, possibly migrated history.
6. `days_to_first_login` matches the event log for only 25 of 897 accounts, so I will recompute it from events rather than trust the field.
7. 165 accounts started within 90 days of the export and cannot yet be labelled retained or churned.

**Open questions:** how to treat pilots, pauses and re-subscriptions in the churn definition; whether the 4.3.0 duplication is a known bug (Priya); whether pre-start events are migrated data (Priya); whether the large ACVs are cents (Selin).
