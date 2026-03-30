Snowflake ELT Pipeline with AWS Glue & Snowpark
An end-to-end ELT pipeline that extracts sales order data from GitHub across three countries, loads it into AWS S3 via AWS Glue, and transforms it through four Snowflake schemas using Snowpark Python.

Architecture
                  GitHub (CSV / Parquet / JSON)
                          │
                          ▼
                     AWS Glue (Python job)
                          │
                          ▼
                      AWS S3 Bucket
                          │
                          ▼
┌──────────────────────────────────────────────────┐
│                 Snowflake (Snowpark)             │
│                                                  │
│        Staging → Raw → Transformed → Curated     |
└──────────────────────────────────────────────────┘


**Objective**
Automate the extraction, transformation, and loading (ELT) of sales data from multiple countries into Snowflake, using AWS Glue for orchestration and Snowpark for in-database transformations.

**Data Sources**
Sales order files are stored in a GitHub repository in different formats per country:
Country Format 
India CSV
USA Parquet
France JSON

**ELT Process**
**1. Data Extraction and Storage — AWS Glue + S3**

AWS Glue runs a Python script that connects to the GitHub repository.
The script extracts sales data for India, USA, and France.
Extracted files are uploaded to an AWS S3 bucket.

**2. Data Loading into Snowflake — Staging Schema**

Snowpark ingests data from S3 into Snowflake.
Files are loaded into their respective tables in the STAGING schema using the COPY INTO command.
After initial validation, data is promoted from STAGING to RAW.

**3. Data Transformation — Transformed Schema**
Transformation logic is applied using Snowpark:

Rearrange and rename columns for consistency across all three country datasets.
Add derived columns where needed.
Split multi-value columns (e.g. MOBILE_MODEL) into separate structured columns.
Union the India, USA, and France datasets into a single consolidated table.
Load the result into the TRANSFORMED schema.

**4. Aggregation and Final Storage — Curated Schema**

Additional aggregation logic is applied using Snowpark.
Final curated datasets are loaded into the CURATED schema, ready for reporting and analytics.


**Snowflake Schema Structure**
Database
├── STAGING       ← Raw files loaded via COPY INTO
├── RAW           ← Validated, schema-aligned tables
├── TRANSFORMED   ← Cleaned, unioned, enriched data
└── CURATED       ← Aggregated, analytics-ready tables

**Tech Stack**
LayerTechnology
Source control
GitHub
OrchestrationAWS Glue (Python shell job)
File storage AWS S3 
Data warehouse Snowflake
Transformation Snowpark Python Notebook
Snowflake Notebooks (Jupyter)
