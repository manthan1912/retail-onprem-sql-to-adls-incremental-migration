# Metadata-Driven Incremental Data Migration Pipeline

## Overview

Built an end-to-end **Azure Data Factory** pipeline to migrate data from an on-premises SQL Server retail data warehouse to **Azure Data Lake Storage Gen2** in Parquet format.

The project uses a **metadata-driven incremental loading framework** with a SQL Server watermark table to load only new records and avoid duplicate ingestion.

## Tech Stack

* Azure Data Factory
* Azure Data Lake Storage Gen2
* SQL Server
* Self-hosted Integration Runtime
* Parquet
* T-SQL

## Architecture

```text
SQL Server RetailDW_Dev
        ↓
Self-hosted Integration Runtime
        ↓
Azure Data Factory
        ↓
ADLS Gen2 / Parquet
```

## Key Features

* Created 10 dimension tables and 10 fact tables in SQL Server.
* Built reusable ADF parent-child pipelines.
* Used Lookup, ForEach, If Condition, Copy Activity, and Stored Procedure Activity.
* Implemented watermark-based incremental loading.
* Stored output files in ADLS Gen2 as Parquet.
* Validated full load, no-change rerun, and incremental load scenarios.

## Pipeline Flow

```text
Parent Pipeline
→ Read watermark table
→ Loop through each source table
→ Call child pipeline

Child Pipeline
→ Get max source watermark
→ Compare with last loaded value
→ Copy new records to ADLS
→ Update watermark table
```

## Folder Structure

```text
raw/retail_dw/dbo/<TableName>/load_date=YYYY-MM-DD/<TableName>_timestamp.parquet
```

## Validation

The pipeline was tested for:

* Initial full load
* Second run with no new data
* Incremental load after inserting a new record
* Watermark update after successful copy

## Result

Successfully built a scalable, reusable, metadata-driven batch ingestion pipeline for on-prem SQL Server to ADLS Gen2 migration.
