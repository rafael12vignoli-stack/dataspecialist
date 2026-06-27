# Data Specialist Coaching — Progress Log

> Persistent memory for Rafael's gamified Data Specialist training program.
> Update this file at the end of every session so progress survives across sessions.

_Last updated: 2026-06-27_

## Player

- **Name:** Rafael
- **Role:** Data Specialist @ YipitData (Corporate Data team) — recently stepped into a more complex role; wants to ramp fast and show a strong learning curve to managers.
- **Background:** ex Data Operations Analyst. Basic Python + SQL (simple automation/debugging). Now needs to test & debug complex data tables using SQL, Python, and PySpark in Databricks.
- **Data worked with:** log data, credit-card data, web-scraping data, large datasets.
- **Business metrics:** GMV, OEU, new cases, activity rates, revenue, auctions, sampling.
- **Setup:** Databricks open in another window to run SQL/PySpark live.

## Current State

- **Level:** 2 — SQL Operator (tested out of Level 1)
- **XP:** 200 / 350 (150 XP to L3 = exactly this lesson's reward)

### Skill stats (0–100)
| Skill | Value |
|---|---|
| SQL foundations | 80 |
| Filtering & aggregation | 75 |
| Investigation instinct | 60 |
| Joins (current focus) | 30 |
| PySpark | 25 |
| Data quality | 25 |
| Window functions | 20 |

### Badges (4/9 earned)
- [x] Calibration complete
- [x] Grain master
- [x] Level 1 cleared
- [x] Sharp instinct
- [ ] Join master (in progress — Lesson 1)
- [ ] Dedup detective
- [ ] Window wizard
- [ ] PySpark pilot
- [ ] Data Specialist (final boss)

## Level Map
1. L1 Data Explorer — cleared
2. **L2 SQL Operator — current**
3. L3 Data Quality Analyst
4. L4 Window Function Navigator
5. L5 Databricks Builder
6. L6 PySpark Converter
7. L7 Metrics Investigator
8. L8 Data Specialist (final boss)

## Calibration Summary
- **Strong:** reads queries, table-grain intuition, wrote a correct grouped SUM (SELECT/WHERE/GROUP BY/SUM/ORDER BY). Good investigation instinct (named duplicates first for a doubled metric).
- **Gaps:** INNER vs LEFT JOIN mechanics; LEFT JOIN + COUNT to include zero-rows; duplicate DETECTION (reached for SELECT DISTINCT instead of GROUP BY / HAVING COUNT(*)>1); window-function mechanics (partition reset + "latest row per group"); PySpark syntax (knows groupBy/agg/sum but scrambles the order).

## Teaching Preferences
- Short, hands-on lessons (10–20 min). NO long lectures. Learns by doing and making mistakes.
- Every lesson: Objective -> Why it matters (Data Specialist context) -> Practical task -> Validation task -> Success criteria.
- Assume no formal SQL/Python/PySpark/Git knowledge; explain terms simply.
- For each new command, cover: (1) what it does, (2) when to use it, (3) when NOT to use it, (4) a real Data Specialist example.
- Build gradually, spaced repetition, checkpoints/mini-bosses/milestone assessments, verify understanding before moving on. Re-explain with analogies if he struggles.
- Connect to real workflows (credit-card/log/web-scrape data, GMV/auctions/sampling).
- Keep the gamified interface: render HUD + level-map widgets every session.

## Active Quest — Lesson 1: Joins
Reward: +150 XP (levels to L3), unlocks "Join master".

Databricks setup cell:
```sql
WITH customers AS (
  SELECT * FROM VALUES (1,'Ana'),(2,'Bruno'),(3,'Carla')
    AS customers(customer_id, name)
),
orders AS (
  SELECT * FROM VALUES (101,1,50),(102,1,30),(103,2,20)
    AS orders(order_id, customer_id, amount)
)
SELECT * FROM customers;
```

- **Task A — INNER JOIN:** list order_id, customer name, amount (one row per order; should be 3 rows).
- **Task B — LEFT JOIN:** order count per customer INCLUDING Carla with 0. Trap: COUNT(*) gives Carla 1; must COUNT the right column so she shows 0.
- **Validation:** predict row counts BEFORE running, then run in Databricks and compare.
- **Success criteria:** Task A returns 3 rows (and explains why Carla is absent); Task B returns Ana 2, Bruno 1, Carla 0, and explains why COUNT(column) beat COUNT(*).

### Status: AWAITING Rafael's Task A + Task B queries and his predictions.

## Interactive Dashboard
- Live app source: `public/index.html` (interactive HUD + Lesson 1 quest grader; saves progress in browser localStorage).
- Hosted via GitHub Pages (workflow `.github/workflows/pages.yml`, deploys on push to the coaching branch).
- Working live URL (no setup needed, via GitHub HTML preview):
  https://htmlpreview.github.io/?https://raw.githubusercontent.com/rafael12vignoli-stack/dataspecialist/claude/data-specialist-coaching-8cdr5x/public/index.html
- Permanent Pages URL (needs one-time owner toggle: Settings -> Pages -> Source: GitHub Actions, then re-run the workflow):
  https://rafael12vignoli-stack.github.io/dataspecialist/
- NOTE: Pages auto-enable via Actions token failed ("Resource not accessible by integration") — first-time Pages enablement must be done by the repo owner in Settings.

## Session Log
- 2026-06-27 — Resumed session. Built interactive HTML dashboard; set up GitHub Pages hosting (Actions workflow). Lesson 1 (Joins) mini-boss validation pending (Rafael says he finished; awaiting his 2 explanations to award +150 XP / L3).
