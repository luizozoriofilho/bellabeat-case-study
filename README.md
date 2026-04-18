# Bellabeat Smart Device Usage Analysis
### Google Data Analytics Certificate — Capstone Project (Case Study 2)

**Analyst:** Luiz Eduardo Ozório  
**Date:** April 2026  
**Tools:** SQL (DBeaver / SQLite), Tableau Public  
**Dataset:** [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) — 35 users, ~60 days (March–May 2016)

---

## Overview

This case study analyzes smart device usage data from FitBit to identify behavioral trends in how consumers use wearable fitness trackers. The insights are then applied to the **Bellabeat App** to inform Bellabeat's marketing strategy.

**Central Question:** What are the trends in smart device usage, and how can they influence Bellabeat's marketing strategy?

---

## Key Findings

| Finding | Result |
|---|---|
| Average daily steps | 7,474 — below the 10,000-step recommended goal |
| Days meeting 10k step goal | Only 31.8% |
| Near-goal days (7,500–9,999 steps) | 15.8% — highest-opportunity group for nudges |
| Largest user segment | Sedentary (31.4% of users) |
| Device usage frequency | 77.1% of users in medium-frequency band (not daily) |
| Most active day of week | Saturday (7,931 avg steps) |
| Least active day | Sunday (6,669 avg steps) |
| Average very active minutes/day | 20:06 min |
| Average sleep duration | 6:58h (~418 min) |

---

## Recommendations

1. **Personalized Movement Reminders** — Segment-aware activity prompts in the Bellabeat App, tailored to each user's activity profile
2. **Near-Goal Step Nudge** — Real-time notifications for users within 2,000 steps of their daily goal
3. **Streak & Consistency Mechanics** — Daily check-in rewards and Sunday-specific content to build consistent usage habits

---

## Dashboard

📊 [View interactive Tableau dashboard](https://public.tableau.com/views/UserSummary/Painel1)

---

## Repository Structure

```
bellabeat-case-study/
│
├── sql/
│   ├── 01_prepare/
│   │   ├── 01_count_users_per_table.sql      # Data coverage check across all source tables
│   │   └── 02_unify_daily_activity.sql       # UNION ALL of daily_activity + daily_activity1
│   │
│   ├── 02_process/
│   │   ├── 01_check_duplicates.sql           # Identify and investigate duplicate Id/Date pairs
│   │   ├── 02_data_quality_checks.sql        # Nulls, zeros, wear time distribution
│   │   ├── 03_create_clean_activity_table.sql # Final cleaned table with correlated subquery dedup
│   │   └── 04_clean_sleep_table.sql          # Sleep dedup, timestamp normalization, derived columns
│   │
│   ├── 03_analyze/
│   │   ├── 01_activity_overview.sql          # Average daily metrics
│   │   ├── 02_step_goal_achievement.sql      # % of days meeting 10k step goal
│   │   ├── 03_user_segmentation.sql          # Activity profile classification per user
│   │   ├── 04_sedentary_vs_active.sql        # Active vs. sedentary time comparison
│   │   ├── 05_weekday_pattern.sql            # Average steps by day of week
│   │   ├── 06_device_usage_consistency.sql   # Days recorded per user / frequency tiers
│   │   └── 07_sleep_vs_activity.sql          # Sleep category vs. activity level cross-analysis
│   │
│   └── 04_export/
│       └── 01_create_export_tables.sql       # Three flat tables exported as CSVs for Tableau
│
├── data/
│   ├── export_user_summary.csv               # 35 rows — one per user (profile + frequency)
│   ├── export_daily.csv                      # 1,351 rows — one per user/day
│   └── export_sleep_activity.csv             # 400 rows — activity + sleep joined
│
└── report/
    └── bellabeat_case_study.pdf              # Full 6-phase analysis report
```

---

## Data Cleaning Summary

| Issue | Action | Records Affected |
|---|---|---|
| Duplicate Id/Date pairs (partial sync) | Kept row with highest TotalSteps (correlated subquery on rowid) | −24 |
| Zero calories + zero steps | Excluded as corrupted/device-off records | −1 |
| Insufficient wear time (< 480 min) | Excluded as non-representative days | ~−16 |
| sleep_day duplicates | Kept record with highest TotalMinutesAsleep | −3 |
| SleepDay timestamp format | Stripped time component for JOIN compatibility | 0 lost |

**Final dataset:** 1,351 activity records / 410 sleep records / 400 joined records

---

## Limitations

- Small sample size (35 users) — may not generalize to Bellabeat's user base
- No demographic data (age, weight, gender) — limits interpretation of calorie and activity metrics
- Data from 2016 — exactly a decade old; behavioral patterns have likely evolved
- SedentaryMinutes includes sleep time — waking sedentary time is ~591 min/day, not 1,009
- Non-linear sleep-activity finding warrants caution without additional demographic context

---

## Tools & Skills Demonstrated

- **SQL (SQLite via DBeaver):** UNION / UNION ALL, correlated subqueries, rowid-based deduplication, date parsing with strftime, CASE WHEN segmentation, window-style aggregations, multi-table JOINs
- **Tableau Public:** Multi-source dashboard, calculated fields, trend lines, consistent color encoding
- **Data Analysis:** ROCCC framework, outlier handling, business-oriented segmentation, limitation documentation

---

*Google Data Analytics Professional Certificate — Capstone Project | April 2026*
