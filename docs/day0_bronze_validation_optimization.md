Bronze Layer Validation & Optimization

Objective-->
Ensure ingestion reliability and optimize Delta storage performance.

Data Validation-->
Performed:
row count validation.
null distribution checks.
timestamp schema verification.
ingestion time boundary validation.
Purpose:
Detect ingestion corruption early.

Performance Optimization-->
Applied:
Delta OPTIMIZE.
ZORDER clustering by user_id.
Table statistics analysis.
Goal:
Improve downstream ML feature queries.

Result-->
Reliable and performance-optimized Bronze dataset ready for Silver feature engineering.
