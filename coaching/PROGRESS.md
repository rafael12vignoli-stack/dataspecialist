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

- **Level:** 3 — Data Quality Analyst (cleared L1 by testing out, L2 by completing Lesson 1: Joins)
- **XP:** 350 / 575 (175 XP to L4 = Lesson 2 reward)

### Skill stats (0–100)
| Skill | Value |
|---|---|
| SQL foundations | 82 |
| Filtering & aggregation | 75 |
| Joins | 70 |
| Investigation instinct | 65 |
| Data quality (current focus) | 25 |
| PySpark | 25 |
| Window functions | 20 |

### Badges (5/9 earned)
- [x] Calibration complete
- [x] Grain master
- [x] Level 1 cleared
- [x] Sharp instinct
- [x] Join master (earned — Lesson 1 Joins complete)
- [ ] Dedup detective (in progress — Lesson 2)
- [ ] Window wizard
- [ ] PySpark pilot
- [ ] Data Specialist (final boss)

## Level Map
1. L1 Data Explorer — cleared
2. L2 SQL Operator — cleared
3. **L3 Data Quality Analyst — current**
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

## COMPLETED — Lesson 1: Joins (+150 XP, L2→L3, 🏅 Join master)
- Task A INNER JOIN = 3 rows; Rafael correctly explained Carla is absent (her customer_id isn't in orders → no match).
- Task B LEFT JOIN: Ana 2, Bruno 1, Carla 0; Rafael correctly explained COUNT(*) counts rows while COUNT(column) counts non-null values, so Carla's NULL order_id → 0.
- Mini-boss validation passed on both questions.

## Active Quest — Lesson 2: Duplicate Detection (Data Quality)
Reward: +175 XP (levels to L4), unlocks "Dedup detective". Targets his calibration gap (reached for SELECT DISTINCT instead of GROUP BY / HAVING COUNT(*)>1).

Databricks setup cell:
```sql
WITH payments AS (
  SELECT * FROM VALUES
    (1,'AnaCard',100),(2,'BrunoCard',50),(3,'CarlaCard',75),
    (2,'BrunoCard',50)   -- duplicated txn
  AS payments(txn_id, card, amount)
)
SELECT * FROM payments;
```

- **Task A — Detect:** `GROUP BY txn_id HAVING COUNT(*) > 1` → returns txn_id 2 (appears 2×). Trap: SELECT DISTINCT can't tell you WHICH/HOW MANY are duplicated.
- **Task B — Quantify GMV impact:** naive SUM = 275, true (deduped) GMV = 225, inflation = 50 (~22% overstatement).
- **Validation:** predict the dup id + the three GMV numbers before running.
- **Success criteria:** correct detection query (GROUP BY + HAVING COUNT(*)>1, not DISTINCT); explains why DISTINCT is insufficient; correct GMV math.

### Status: ACTIVE — Lesson 2 loaded into dashboard. Awaiting Rafael's detection query + predictions.

### Next up (preview): L3 may include a spaced-repetition mini-check, then L4 Window Functions (ROW_NUMBER / PARTITION BY, "latest row per group" — another calibration gap).

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
- 2026-06-27 — Lesson 1 mini-boss PASSED. Awarded +150 XP → Level 3, unlocked Join master, Joins 30→70, Investigation 60→65. Advanced dashboard to Lesson 2 (Duplicate Detection). HUD now lives in the web app — stop pasting ASCII HUDs in chat.
