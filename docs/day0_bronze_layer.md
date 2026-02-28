Day 0 — Bronze Layer Construction

Objective-->
Establish reliable raw data storage for ecommerce behavioural analytics.

Bronze Foundation-->
Combined October and November datasets.
Unified behavioural events (~100M records).

Infrastructure Constraint-->
Public DBFS root access is disabled in Databricks Serverless.
Bronze tables implemented using Unity Catalog Managed Delta Tables.

Engineering Enhancements-->
Converted event_time → timestamp.
Added ingestion_time metadata.
Removed fully empty records.

Storage-->
Registered Delta Tables:
workspace.ecommerce.bronze_events_raw
workspace.ecommerce.bronze_events_clean

RAW Bronze Purpose-->
Used for:- Ensures exact source preservation , rollback , auditability ,
processing capability , efficient debugging 

CLEAN Bronze Purpose--->
Featured :-	○ System-level structural enhancements.
		        ○ Timestamp normalization.
	        	○ Ingestion metadata addition.
	         	○ No business logic transformation applied.
Ensures :- Analytics ; Silver layer creation ; ML features ; 
Reproducibility ; Data lineage tracking ; Production-ready foundation
Because:- timestamp fixed ; ingestion tracked.
Safer working copy.

Why Delta?-->
Explain:- ACID transactions ; Time travel ; Schema enforcement.
