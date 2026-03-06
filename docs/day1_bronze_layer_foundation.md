Day 1 — Bronze Layer Build UP


1. Architectural Context
   
The Bronze layer represents the ingestion boundary of the ecommerce analytics platform.
Its purpose is not analytical transformation, but controlled preservation of behavioural event data within a governed, ACID-compliant transactional storage system.

The objective of this stage is not analytical transformation, but controlled data preservation combined with operational reliability and reproducibility guarantees aligned with Medallion Architecture principles.This layer establishes the  foundation upon which Silver and Gold layers depend.

Platform Architecture
Kaggle Dataset
        │
        ▼
Databricks Ingestion Pipeline
        │
        ▼
Unity Catalog
        │
        ▼
workspace.ecommerce
        │
        ├── bronze_events_raw
        └── bronze_events
                partitioned by event_date

                

2. Design Principles

The Bronze implementation was guided by the following principles:

Preserve source fidelity.
Avoid premature business logic transformation.
Guarantee reproducibility.
Enable auditability and rollback.
Align with modern cloud-native data platform standards.
This ensures that all downstream processing remains deterministic and traceable to an immutable ingestion state.


3. Dataset Consolidation

Behavioural event datasets from October and November were consolidated into a unified ingestion stream.

Resulting dataset:
~110 million interaction records.(~86% brand attribution completeness. ~67% category metadata availability.)
Standardized schema alignment across ingestion sources.
Event-level granularity (user behaviour, product interactions, session activity).

No analytical filtering or enrichment was applied at this stage.


4. Platform Constraint and Architectural Adaptation

Databricks Serverless environments disable public DBFS root access as part of their security architecture.
Legacy path-based writes (e.g., /delta/...) are therefore unsupported.

To comply with modern platform governance:
Unity Catalog managed Delta tables were adopted.
Storage responsibilities were delegated to catalog-governed infrastructure.

This ensures:
Centralized metadata management
Fine-grained access control
Persistent table registration
Platform-native discoverability


5. Bronze Layer Implementation Strategy

Bronze consists of two controlled datasets:

5.1 RAW Bronze (workspace.ecommerce.bronze_events_raw)

Purpose:
Preserve exact ingestion output.
Serve as immutable reference state.
Support rollback and reprocessing.
Provide authoritative audit baseline.

This dataset is intentionally unmodified beyond ingestion mechanics.The RAW Bronze table is treated as append-only and operationally immutable,serving as the authoritative ingestion snapshot.

5.2 CLEAN Bronze (workspace.ecommerce.bronze_events)

Purpose:
Provide structurally stable working copy.
Prepare dataset for downstream transformation.
Introduce operational metadata without altering semantics.

Enhancements applied:
event_time converted to timestamp datatype.
ingestion_time metadata column introduced.
Fully empty records removed.

No business logic or domain filtering was applied.


6. Transactional Storage Selection — Delta Lake

Delta Lake was selected to introduce database-grade guarantees into the data lake.Delta Lake enables reliable ingestion by introducing transactional guaranteesinto the data lake layer, ensuring that large-scale event datasets can be written, queried, and versioned safely without corruption or partial updates.

Capabilities leveraged:---
ACID Transactions : Prevent partial writes and ensure consistency under concurrent operations.
Time Travel : Enable historical snapshot retrieval and deterministic rollback.
Schema Enforcement : Protect against unintended structural drift during ingestion evolution.

These guarantees elevate the storage layer from passive file storage to governed transactional infrastructure.


7. Operational Guarantees Established

The Bronze layer now provides:
Version-controlled dataset history
Snapshot isolation
Deterministic dataset reconstruction
Rollback capability
Ingestion audit traceability

This enables safe experimentation, debugging, and regulatory-grade reproducibility.


8. Boundary Definition Within Medallion Architecture

Bronze intentionally avoids:
Business aggregations
Deduplication based on domain rules
Feature engineering
Analytical filtering
Such transformations are deferred to Silver and Gold layers.

This separation preserves clean architectural boundaries and prevents irreversible mutation of source data.


9. Governance and Future Extensibility

Design decisions support future platform evolution, including:
Incremental ingestion workflows
Schema evolution handling
Data quality enforcement in Silver
Feature store integration
Lineage observability tooling
The Bronze layer is therefore scalable, not static.

10. Physical Storage Layout

The Bronze dataset is partitioned by event_date.
This partitioning strategy enables partition pruning during query execution,
reducing unnecessary data scanning and improving performance for time-bounded queries.

Example layout:
bronze_events/
  event_date=2019-10-01/
  event_date=2019-10-02/
  ....

12. Dataset Integrity Validation
The Bronze layer was validated through several integrity checks:
Row count verification
Event distribution validation
Schema inspection
Delta transaction history verification

Final dataset size: 109,950,743 events

Event distribution:
view        ~104M
cart         ~3.9M
purchase     ~1.6M


11. Outcome

Day 1 successfully transitioned raw ingestion outputs into governed, ACID-compliant Delta tables aligned with enterprise data engineering practices.
The platform now supports reliable downstream transformation while preserving source transparency, operational resilience, and reproducible state management.
