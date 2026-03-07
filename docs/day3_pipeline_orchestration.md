# 📄 Day 3 — Pipeline Orchestration (Databricks Jobs)

## Overview

Day 3 introduces orchestration capabilities to the ecommerce analytics platform.
The pipeline notebook is converted into a **parameterized execution module** capable of running different processing layers depending on runtime configuration.

This notebook is deployed as a **Databricks Job**, allowing the pipeline to run through the platform’s workflow engine instead of manual notebook execution.

This step transitions the project from an interactive development workflow into an **automated data pipeline environment**.

---

# Objective

The goal of this stage is to introduce workflow management that allows the data pipeline to:

* Execute specific pipeline layers dynamically
* Run without manual intervention
* Integrate with Databricks job orchestration
* Support automated scheduling

This provides the foundation for a scalable data platform architecture.

---

# Pipeline Architecture

The orchestration layer acts as a controller that determines which processing stage should run.

Pipeline layers currently implemented:

| Layer  | Purpose                                        |
| ------ | ---------------------------------------------- |
| Bronze | Raw data ingestion and storage                 |
| Silver | Feature engineering and user-level aggregation |

The orchestration workflow routes execution to the appropriate stage based on runtime configuration.

Conceptually, the execution flow follows this structure:

Databricks Job
→ Pipeline Controller
→ Selected Pipeline Layer
→ Delta Lake Table Output

This modular design allows individual layers to run independently while remaining part of the same pipeline framework.

---

# Parameterized Pipeline Execution

To support flexible orchestration, the pipeline uses runtime parameters that determine which processing layer executes.

This allows a single notebook to function as a **multi-stage pipeline controller**.

Example execution scenarios include:

| Runtime Parameter | Executed Stage               |
| ----------------- | ---------------------------- |
| bronze            | Raw ingestion pipeline       |
| silver            | Feature engineering pipeline |

This approach reduces code duplication and enables reusable pipeline components.

---

# Modular Pipeline Design

The pipeline architecture separates processing responsibilities into independent pipeline stages.

### Bronze Layer

The Bronze layer serves as the **raw ingestion boundary** of the platform.

Responsibilities include:

* Ingesting raw ecommerce event data
* Persisting raw records into Delta Lake
* Establishing the foundational storage layer of the data platform

This layer preserves the original event structure while ensuring data durability and traceability.

---

### Silver Layer

The Silver layer performs **data transformation and feature engineering**.

Responsibilities include:

* Reading event-level data from the Bronze layer
* Aggregating behavioral metrics at the user level
* Generating analytical features suitable for downstream analytics and machine learning

The resulting dataset provides a clean and structured representation of user activity.

---

# Databricks Job Orchestration

To operationalize the pipeline, the notebook is executed through the **Databricks Jobs system**.

This enables:

* Parameterized pipeline execution
* Compute provisioning through serverless infrastructure
* Job monitoring and execution logs
* Automated scheduling capabilities

Each job run executes the pipeline controller with defined runtime parameters, allowing specific pipeline stages to be triggered.

---

# Job Configuration

The orchestration job is configured with the following parameters:

| Configuration     | Value                    |
| ----------------- | ------------------------ |
| Job Name          |Silver Pipeline  |
| Execution Type    | Notebook Task            |
| Compute           | Serverless               |
| Runtime Parameter | Pipeline layer selection |

This configuration enables controlled execution of individual pipeline layers within the platform.

---

# Execution Monitoring

Databricks provides detailed run metadata for each pipeline execution.

Available monitoring information includes:

* Job ID
* Task Run ID
* Execution duration
* Execution logs
* Task success status

These details provide visibility into pipeline performance and operational reliability.

Execution logs also serve as **verifiable evidence that the pipeline executed successfully within the workflow system**.

---

# Pipeline Scheduling

To simulate production pipeline behavior, the job is configured with a **daily execution schedule**.

Schedule configuration:

| Property       | Value    |
| -------------- | -------- |
| Frequency      | Daily    |
| Execution Time | 02:00 AM |

Automated scheduling ensures the pipeline processes newly available data on a regular basis without manual intervention.

This design mirrors real-world production data pipelines where workflows are triggered by time-based schedules.

---

# Operational Benefits

Introducing orchestration provides several key advantages.

### Automation

Pipeline execution no longer requires manual notebook interaction.

### Reusability

A single pipeline controller can trigger multiple pipeline stages.

### Observability

Job run metadata allows monitoring of pipeline execution and performance.

### Scalability

New pipeline stages can be added without restructuring the orchestration layer.

---

# Future Extensions

The current architecture supports expansion into additional processing layers.

Planned extensions include:

* Gold layer for business-ready analytics datasets
* Dashboard-ready aggregated tables
* Machine learning feature sets

All future layers will be integrated into the same orchestration framework.

---

# Summary

Day 3 introduces workflow orchestration to the ecommerce analytics platform by deploying the pipeline notebook as a Databricks Job.

Key outcomes of this stage include:

* Parameterized pipeline execution
* Modular pipeline architecture
* Automated workflow orchestration
* Daily scheduled pipeline execution

This step represents the transition from exploratory notebook development to an **operational data pipeline capable of automated execution within a managed workflow system**.
