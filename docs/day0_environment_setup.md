Day 0 — Environment Setup & Cloud Data Ingestion


1. Architectural Context
Day 0 establishes the foundational cloud infrastructure required for governed dataset onboarding within a Databricks Serverless environment. The objective of this phase is to prepare secure compute execution, authenticated external data acquisition, and persistent catalog-governed storage prior to dataset processing workflows.


2. Objectives

Environment initialization focused on:
enabling distributed Spark execution.
establishing secure external dataset authentication.
provisioning Unity Catalog governed storage.
implementing reproducible dataset acquisition.
These steps ensure reliable onboarding of large external datasets into the analytics platform.


3. Compute Environment Initialization
A Databricks Serverless notebook environment was provisioned as the execution boundary for ingestion workflows.

Actions performed:
Initialized Serverless Spark runtime.
Verified notebook execution capability.
Installed Kaggle CLI dependency:
%pip install kaggle
Serverless compute provides automatic scaling and removes manual cluster lifecycle management.


4. Secure Kaggle API Authentication
Automated dataset acquisition required authenticated Kaggle API access.

Credential Configuration
Authentication was configured using:
secure upload of kaggle.json.
credential extraction into runtime configuration path.
environment variable setup to prevent credential exposure within notebooks.
This approach separates sensitive credentials from application logic.

Connectivity Verification
Authentication and connectivity were validated using: kaggle datasets list

Successful execution confirmed:
valid API authentication
CLI readiness.
network accessibility.
Verification prevents ingestion failures during large dataset transfer operations.


5. Unity Catalog Configuration
Storage resources were provisioned using Unity Catalog to comply with Databricks Serverless governance requirements.

5.1 Schema Creation

Created schema: workspace.ecommerce
The schema serves as the logical namespace for Medallion architecture datasets.

5.2 Persistent Volume Provisioning
Created managed volume: workspace.ecommerce.ecommerce_data

Volume characteristics:
persistent across compute restarts.
governed by catalog permissions.
compatible with Serverless security architecture.
Volume structure was verified to confirm accessibility and correct allocation.


6. Automated Dataset Ingestion
The ecommerce behavioural dataset (~4.29GB) was acquired programmatically using the Kaggle CLI.

Dataset ingestion process:
download executed within compute environment.
dataset stored directly inside Unity Catalog volume.
transfer completion verified via filesystem inspection.
Programmatic acquisition ensures reproducible onboarding and removes dependency on manual uploads.


7. Operational Guarantees Established

Environment setup establishes:
authenticated external data acquisition.
governed persistent storage placement.
distributed Spark execution readiness.
reproducible ingestion configuration.
These guarantees provide a stable infrastructure baseline for Bronze layer construction.


8. Platform Constraints and Compliance
Databricks Serverless environments restrict usage of public DBFS root storage.
All ingestion artifacts were therefore placed inside Unity Catalog managed storage to maintain governance compliance and persistence guarantees.

9. Outcome
Day 0 environment initialization successfully established secure compute execution, authenticated dataset access, and governed storage infrastructure.
The platform is prepared for large-scale ingestion validation and Delta-based Bronze layer implementation.

10. Status
Day 0 infrastructure setup completed successfully.

Environment readiness includes:
Serverless Spark execution.
Kaggle API authentication.
Unity Catalog schema provisioning.
Persistent volume storage.
Automated dataset onboarding capability.
