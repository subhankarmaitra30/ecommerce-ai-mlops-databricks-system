Day 2 — Silver Layer Feature Engineering

1) Objective:--
Transform raw behavioral event data stored in the Bronze layer into structured user-level features suitable for downstream machine learning and analytics.
The goal of this stage is to convert event-level logs into entity-level behavioral features.

2)Dataset Source:--

Bronze Layer Table: workspace.ecommerce.bronze_events

Characteristics:
~110 million user interaction events
event-level granularity
partitioned by event_date

Columns:
event_time
event_type
product_id
category_id
category_code
brand
price
user_id
user_session
ingestion_time
event_date


3)Silver Layer Design
Silver layer aggregates behavioral signals at user level.

Target table: workspace.ecommerce.silver_user_features
Each row represents one user profile.

Schema:
user_id
total_events
total_purchases
total_spent
avg_price
purchase_rate
feature_generation_time

4) Feature Engineering Logic

User behavioral features are derived using Spark aggregation.

Example logic:groupBy(user_id)

Compute:

Feature                                          	Description
total_events	                   total interactions performed by the user
total_purchases	                           number of purchase events
total_spent                                  	total money spent
avg_price                            	average price of interacted items
purchase_rate	                            purchases / total_events


5)Data Quality Handling

Two key data cleaning steps were applied:
Missing values in price-based features replaced with zero
total_spent → 0
avg_price → 0

Feature generation timestamp added for lineage tracking.

6)Final Storage

The engineered dataset is stored as a Delta table.
workspace.ecommerce.silver_user_features

Benefits:
ACID transaction support
scalable metadata
compatible with ML pipelines


7)Medallion Architecture Alignment

This project follows the Medallion Data Architecture.

Bronze → raw event ingestion
Silver → cleaned & structured features
Gold → ML predictions and recommendations

8) Output Characteristics

The Silver dataset now provides a machine learning ready feature store.

Typical usage:
customer purchase prediction
recommendation systems
behavioral segmentation
