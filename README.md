# econ3916-lab11-sql
# SQL for Economists — Joins, Windows and the Cardinality Trap

**Objective:** I translated pandas operations into SQL with DuckDB and tested where the two give different answers, especially joins that multiply rows and missing values.

## Methodology

- Translated select, filter, sort, groupby and merge from pandas into SQL with DuckDB, and checked that both returned the same rows.
- Joined 600 customers to their orders with a one-to-many LEFT JOIN, then ran the same Welch t-test of East against West revenue before and after the join.
- Used window functions (AVG OVER PARTITION BY, RANK) to attach each region's average and each customer's rank to every row without collapsing the table.
- Compared SQL's three-valued logic with pandas on a small survey with missing incomes.
- Queried a public Parquet file of about 3 million NYC taxi trips over HTTP without downloading it, reading only the columns the query needed.
- Asked an AI to translate three of my pandas steps into SQL, revised my prompt once, and compared the row counts of its queries with my pandas results.

## Key Findings

- **The cardinality trap.** The join turned 600 customer rows into 3,612 rows. On the same revenue figures, the Welch t-test went from t = 0.07 (p = 0.9413) to t = 0.71 (p = 0.4749). The repeated rows shrank the standard error and moved the difference, and the join gave no error or warning.
- **NULL is not a value.** `WHERE income != 50000` kept 3 of 5 survey rows, because SQL treats a comparison with a missing value as unknown and WHERE drops it. The same filter in pandas kept all 5.
- **Remote queries.** DuckDB read only the three columns the taxi query used and returned six rows, without downloading the whole file.
- **Checking AI's SQL.** The AI's first translation of the survey filter matched SQL's rules but not my pandas result: 3 rows instead of 5. Once my revised prompt said how to handle missing incomes, all three row counts matched.
