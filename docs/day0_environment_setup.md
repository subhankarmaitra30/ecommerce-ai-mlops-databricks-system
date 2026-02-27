Day 0 – Environment Setup & Cloud Data Ingestion

1. Databricks Serverless Compute Initialization
Created serverless notebook environment.
Verified runtime execution.
Installed Kaggle CLI using %pip install kaggle.

2. Secure Kaggle API Authentication
Uploaded kaggle.json.
Extracted credentials securely.
Configured environment variables.
Verified API connection via kaggle datasets list.

3. Unity Catalog Configuration
Created schema: workspace.ecommerce
Created persistent volume: workspace.ecommerce.ecommerce_data
Verified volume structure.

4. Automated Dataset Ingestion
Downloaded 4.29GB ecommerce dataset directly from Kaggle.
Stored inside Unity Catalog volume.
Confirmed successful transfer.

Status: Day-0 Infrastructure Completed Successfully.
