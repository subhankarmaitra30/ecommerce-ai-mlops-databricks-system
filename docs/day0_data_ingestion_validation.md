Day 0 — Ecommerce Dataset Ingestion & Validation

1. Architectural Context
Day 0 ingestion establishes the external data onboarding boundary of the ecommerce analytics platform.
This stage integrates large-scale behavioural event data into Databricks Serverless using governed Unity Catalog storage. The objective is to ensure reliable acquisition, structural validation, and platform compatibility prior to Bronze layer construction.


2. Objectives

The ingestion workflow was designed to:
automate external dataset acquisition.
store artifacts within Unity Catalog–governed storage.
validate dataset structure before distributed processing.
ensure compatibility with Medallion Architecture implementation.
Early validation reduces downstream correction effort during Silver and Gold layer development.


3. Dataset Source

Source:

Kaggle dataset — ecommerce-behavior-data-from-multi-category-store
The dataset contains anonymized ecommerce behavioural interaction records 
representing:
product views
cart additions
purchase events

These events form the analytical foundation for user behaviour modeling and recommendation workflows.


4. Platform Environment

All ingestion operations were performed using:
Databricks Serverless compute
Apache Spark distributed processing
Unity Catalog volumes for persistent storage

Public DBFS root access is disabled in Serverless environments.
All dataset artifacts were therefore stored within Unity Catalog–managed volumes to ensure governance compliance and persistence across cluster restarts.

5. Data Engineering Workflow

5.1 Dataset Acquisition
The dataset was downloaded programmatically using the Kaggle API.

This ensures:
reproducible data acquisition.
elimination of manual upload dependency.
consistent onboarding across environments.

5.2 Governed Storage Placement

Downloaded files were stored in a Unity Catalog Volume.

This provides:
persistent storage independent of compute lifecycle.
centralized access governance.
catalog-managed visibility.

5.3 Archive Extraction

The compressed dataset archive was extracted using controlled shell execution within the workspace.
Extraction enabled compatibility with Spark file readers while preserving file integrity.

5.4 Runtime Metadata Refresh

Runtime restart was performed following extraction to refresh filesystem metadata.
This ensures newly extracted files are discoverable by Spark within the Serverless environment.

6. Distributed Data Loading

Datasets were loaded using Spark distributed CSV ingestion.

Example: spark.read.csv(...)
The October dataset was successfully loaded.
Dataset scale:
approximately 40 million event records.

Distributed ingestion enables scalable parsing while maintaining schema transparency.

7. Schema Validation

Schema inspection confirmed the expected structure.

Verified columns:
event_time
event_type
product_id
category_code
brand
price
user_id
user_session

Validation ensures compatibility with downstream Bronze and Silver workflows.


8. Dataset Characteristics

Initial inspection identified:
Nullable values in brand and category_code.
Multi-stage behavioural signals including view, cart, and purchase events.

These characteristics informed the Bronze layer strategy prioritizing source preservation over early transformation.


9. Outcome

Day 0 ingestion successfully:
automated external dataset onboarding.
stored artifacts in governed Unity Catalog storage.
validated schema and dataset scale.
prepared the dataset for Bronze layer construction.

The platform now contains a reproducible, structurally verified behavioural dataset aligned with Medallion Architecture principles.
