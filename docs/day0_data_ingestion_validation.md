Day 0 — Ecommerce Dataset Ingestion & Validation:---
Objective -->
Establish automated ingestion pipeline and validate large-scale ecommerce behavioral dataset using Databricks Serverless Spark.

Dataset Source-->
Kaggle Dataset: ecommerce-behavior-data-from-multi-category-store.

Data Engineering Steps-->
Downloaded dataset using Kaggle API.
Stored dataset in Unity Catalog Volume.
Extracted compressed archive using shell execution.
Restarted runtime to refresh metadata.

Spark Data Loading-->
spark.read.csv(...)

Loaded October dataset (~40M events).

Schema Validation-->
Confirmed expected columns:
event_time 
event_type
product_id
category_code
brand
price
user_id
user_session.

Key Observations-->
Nullable brand and category_code columns observed.
Dataset contains view/cart/purchase behaviour.


Status: Day-0 completed successfully.....
