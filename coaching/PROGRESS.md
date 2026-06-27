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

- **Level:** 5 — Databricks Builder (cleared L1 test-out, L2 Joins, L3 Dedup, L4 Window Functions)
- **XP:** 775 / 1075 (300 XP to L6 = Lesson 4 reward)

### Skill stats (0–100)
| Skill | Value |
|---|---|
| SQL foundations | 86 |
| Filtering & aggregation | 75 |
| Joins | 70 |
| Data quality | 70 |
| Window functions | 70 |
| Investigation instinct | 70 |
| PySpark (current focus) | 25 |

### Badges (7/9 earned)
- [x] Calibration complete
- [x] Grain master
- [x] Level 1 cleared
- [x] Sharp instinct
- [x] Join master (Lesson 1 Joins)
- [x] Dedup detective (Lesson 2 Duplicate Detection)
- [x] Window wizard (Lesson 3 Window Functions)
- [ ] PySpark pilot (in progress — Lesson 4)
- [ ] Data Specialist (final boss)

## Level Map
1. L1 Data Explorer — cleared
2. L2 SQL Operator — cleared
3. L3 Data Quality Analyst — cleared
4. L4 Window Function Navigator — cleared
5. **L5 Databricks Builder — current**
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

## COMPLETED — Lesson 3: Window Functions (+250 XP, L4→L5, 🏅 Window wizard)
- First attempt erred: he added GROUP BY case_id + HAVING rank=1 (recency bias from Lesson 2). Taught logical order of execution (window functions run in SELECT, after WHERE/GROUP BY/HAVING) → can't filter rn in same query.
- Second attempt CORRECT: ROW_NUMBER() OVER (PARTITION BY case_id ORDER BY updated_at DESC), no GROUP BY for Task A; CTE + WHERE rank=1 for Task B. Predictions 5/3 and 2/closed/closed all correct.
- Coaching: avoid alias `rank` (reserved word / built-in fn) — use `rn`; QUALIFY is the Databricks shortcut.

## Active Quest — Lesson 4: PySpark DataFrames (select & filter)
Reward: +300 XP (levels to L6), unlocks "PySpark pilot". First PySpark lesson — SQL→PySpark mental map.

Databricks setup cell (Python):
```python
from pyspark.sql import functions as F
data = [(101,1,50),(102,1,30),(103,2,20)]
df = spark.createDataFrame(data, ["order_id","customer_id","amount"])
df.show()
```

- **Task A — Translate:** SELECT order_id, amount FROM orders WHERE amount>25 → `df.filter(F.col("amount")>25).select("order_id","amount").show()`. Returns 2 rows: 101, 102 (103 dropped). Trap: PySpark is lazy — need an action (.show()).
- **Task B — withColumn:** add fee = 10% of amount → `df.withColumn("fee", F.col("amount")*0.1)`. fee on order 101 = 5.0.
- **Validation:** predict rows (2) + order_ids (101,102); predict fee on 101 (5.0).
- **Success criteria:** correct .filter/.where + .select + action; correct withColumn with *0.1; understands lazy vs action.

### Status: ACTIVE — Lesson 4 loaded into dashboard. Awaiting Rafael's PySpark code + predictions.

### Next up (preview): L6 PySpark Converter (translate SQL GROUP BY/agg → PySpark df.groupBy().agg() — his known gap: scrambles groupBy/agg/sum order), L7 Metrics Investigator (debug a doubled-GMV case = mini-boss), L8 Data Specialist final boss.

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
- 2026-06-27 — Lesson 3: first attempt erred (GROUP BY/HAVING on a window) — taught logical execution order; second attempt PASSED. Awarded +250 XP → Level 5, unlocked Window wizard, Window functions 20→70, Investigation 68→70. Advanced dashboard to Lesson 4 (PySpark DataFrames). Note: he asked "pq da errado?" in Portuguese — comfortable in PT, course running in EN.
