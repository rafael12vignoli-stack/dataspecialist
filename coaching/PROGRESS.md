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

- **Level:** 4 — Window Function Navigator (cleared L1 test-out, L2 Joins, L3 Duplicate Detection)
- **XP:** 525 / 775 (250 XP to L5 = Lesson 3 reward)

### Skill stats (0–100)
| Skill | Value |
|---|---|
| SQL foundations | 84 |
| Filtering & aggregation | 75 |
| Joins | 70 |
| Data quality | 70 |
| Investigation instinct | 68 |
| Window functions (current focus) | 20 |
| PySpark | 25 |

### Badges (6/9 earned)
- [x] Calibration complete
- [x] Grain master
- [x] Level 1 cleared
- [x] Sharp instinct
- [x] Join master (Lesson 1 Joins)
- [x] Dedup detective (Lesson 2 Duplicate Detection)
- [ ] Window wizard (in progress — Lesson 3)
- [ ] PySpark pilot
- [ ] Data Specialist (final boss)

## Level Map
1. L1 Data Explorer — cleared
2. L2 SQL Operator — cleared
3. L3 Data Quality Analyst — cleared
4. **L4 Window Function Navigator — current**
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

## COMPLETED — Lesson 2: Duplicate Detection (+175 XP, L3→L4, 🏅 Dedup detective)
- Detection query correct: `GROUP BY txn_id HAVING COUNT(txn_id) > 1` (used GROUP BY/HAVING, NOT DISTINCT — calibration gap closed).
- txn_id 2 appears 2×; naive SUM 275, true GMV 225, inflation 50. All correct.
- Coaching given: COUNT(col)=COUNT(*) only when no NULLs (spaced-rep tie to Lesson 1); real dupes may need composite-key GROUP BY.

## Active Quest — Lesson 3: Window Functions — latest row per group
Reward: +250 XP (levels to L5), unlocks "Window wizard". Targets calibration gap (window mechanics: partition reset + "latest row per group").

Databricks setup cell:
```sql
WITH case_status AS (
  SELECT * FROM VALUES
    (10,'open',DATE'2026-01-01'),(10,'in_review',DATE'2026-01-03'),(10,'closed',DATE'2026-01-05'),
    (20,'open',DATE'2026-01-02'),(20,'closed',DATE'2026-01-04')
  AS case_status(case_id, status, updated_at)
)
SELECT * FROM case_status;
```

- **Task A — Rank:** `ROW_NUMBER() OVER (PARTITION BY case_id ORDER BY updated_at DESC) AS rn`. Returns all 5 rows; case 10 'open' = rn 3.
- **Task B — Latest per case:** wrap in CTE, `WHERE rn = 1` (or `QUALIFY`). Returns 2 rows: case 10 closed, case 20 closed. Trap: can't put ROW_NUMBER() in WHERE directly.
- **Validation:** predict total rows (5), rn of 'open' (3), then rows (2) + latest statuses (both closed).
- **Success criteria:** correct ROW_NUMBER/PARTITION BY/ORDER BY DESC; correctly filters rn=1 via CTE or QUALIFY; explains partition reset.

### Status: ACTIVE — Lesson 3 loaded into dashboard. Awaiting Rafael's ranking query + predictions.

### Next up (preview): L5 Databricks Builder (PySpark DataFrame basics), then L6 PySpark Converter (translate SQL→PySpark groupBy/agg), L7 Metrics Investigator (debug a doubled GMV case = mini/final boss).

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
- 2026-06-27 — Lesson 2 PASSED (query + all predictions correct). Awarded +175 XP → Level 4, unlocked Dedup detective, Data quality 25→70. Advanced dashboard to Lesson 3 (Window Functions). Rafael is moving fast and getting everything right on first try.
