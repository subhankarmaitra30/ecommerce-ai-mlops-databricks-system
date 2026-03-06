Day1— Ecommerce Dataset Ingestion & Validation

1. Architectural Context
Day 1 ingestion establishes the external data onboarding boundary of the ecommerce analytics platform.
This stage integrates large-scale behavioural event data into Databricks Serverless using governed Unity Catalog storage. The objective is to ensure reliable acquisition, structural validation, and platform compatibility prior to Bronze layer construction.

2. Platform Data Flow
External Source
(Kaggle Dataset)
        │
        ▼
Databricks Serverless Compute
        │
        ▼
Unity Catalog Volume
(/Volumes/workspace/ecommerce/ecommerce_data)
        │
        ▼
Spark Distributed Ingestion
        │
        ▼
Schema Inspection & Validation
        │
        ▼
Dataset Ready for Bronze Layer Construction



3. Objectives

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


5. Platform Environment

All ingestion operations were performed using:
Databricks Serverless compute
Apache Spark distributed processing
Unity Catalog volumes for persistent storage

Public DBFS root access is disabled in Serverless environments.
All dataset artifacts were therefore stored within Unity Catalog–managed volumes to ensure governance compliance and persistence across cluster restarts.


6. Data Engineering Workflow

6.1 Dataset Acquisition
The dataset was downloaded programmatically using the Kaggle API.

This ensures:
reproducible data acquisition.
elimination of manual upload dependency.
consistent onboarding across environments.

6.2 Governed Storage Placement

Downloaded files were stored in a Unity Catalog Volume.

This provides:
persistent storage independent of compute lifecycle.
centralized access governance.
catalog-managed visibility.

6.3 Archive Extraction

The compressed dataset archive was extracted using controlled shell execution within the workspace.
Extraction enabled compatibility with Spark file readers while preserving file integrity.

6.4 Runtime Metadata Refresh

Runtime restart was performed following extraction to refresh filesystem metadata.
This ensures newly extracted files are discoverable by Spark within the Serverless environment.

7. Reproducibility Principles--
The ingestion pipeline was designed for deterministic execution.
Key characteristics:
• automated dataset acquisition using Kaggle API
• environment-independent storage via Unity Catalog volumes
• distributed Spark ingestion for scalable parsing
• deterministic schema inspection prior to transformation

These principles ensure the ingestion workflow can be executed reliably across development, testing, and production environments.



8. Distributed Data Loading

Datasets were loaded using Spark distributed CSV ingestion.

Example: spark.read.csv(...)
The October dataset was successfully loaded.
Dataset scale:
approximately 40 million event records.

Distributed ingestion enables scalable parsing while maintaining schema transparency.

9. Schema Validation

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


10. Dataset Characteristics

Initial inspection identified:
Nullable values in brand and category_code.
Multi-stage behavioural signals including view, cart, and purchase events.

The ecommerce behavioural dataset represents large-scale user interaction logs.

Dataset scale:
October dataset:   ~42 million events
November dataset:  ~67 million events
Combined dataset:  ≈109,950,743 behavioural events

Event distribution characteristics:
view events      ≈ 104M
cart events      ≈ 3.9M
purchase events  ≈ 1.6M

These characteristics informed the Bronze layer strategy prioritizing source preservation over early transformation.



11.Data Validation Strategy

Dataset integrity was verified through multiple validation steps.

Validation checks included:

Schema validation
Column nullability inspection
Event distribution verification
Dataset scale confirmation

These checks ensure the dataset integrity before downstream transformation within the Bronze layer.

12. Key Engineering Decisions

Several architectural decisions were made during ingestion design:

      Decision	                                                  Reason
  Use Kaggle API                              	       reproducible ingestion
  Use Unity Catalog Volumes	                           governed persistent storage
  Avoid early transformations	                     preserve raw behavioural signals
  Perform schema validation early	                 reduce downstream correction effort


13.Outcome

The Day 1 ingestion pipeline successfully established the external data onboarding boundary for the ecommerce analytics platform.

Key achievements:
• automated dataset acquisition from Kaggle
• governed storage within Unity Catalog volumes
• distributed Spark ingestion of large-scale event data
• structural validation of dataset schema and characteristics

The platform now contains a reproducible behavioural dataset foundation ready for Bronze layer construction within the Medallion Architecture framework.
