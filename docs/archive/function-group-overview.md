# Function Group Overview

## Summary
Transformation Flow (`txflow`) functions are organised into logical groups depending on the type of activity they support.  

These groups are either aligned to the sequential stages in a typical data pipeline (`ingest` → `profile` → `transform` → `export` → `monitor`) or groups which support data administration (`admin`) or the development of new functionality and flows (`work_in_progress`, `transformations`)

## Function Groups

| schema_name | schema_ref | schema_description |
|---|---|---|
| ingest | txflow.ingest | Functions which support setup of data ingestion points into BigQuery. |
| profile | txflow.profile | Functions which support profiling of data at every point in the transformation flow. |
| transform | txflow.transform | Transform functions (`txflow.transform`) form the core of the Transformation Flow framework. They automate and simplify common data transformation patterns and can be used to either generate SQL code for a single transformation stage (for inspection, execution or view creation) or composed into a sequential flow to build more complex multi-stage transformations. |
| monitor | txflow.monitor | Functions which support monitoring of data ingestion, transformation, export and usage. |
| export | txflow.export | Functions which support post-transformation data export tasks. |
| blocks | txflow.blocks | Functions which support composition of atomic SQL transformations into transformation flows. |
| transformations | txflow.transformations | Complete transformation flows for specific use cases and data sources, packaged as functions which can then be deployed to different datasets, or used as foundations to implement similar transformations for modified use cases. |
| admin | txflow.admin | Functions which support automation of common general administrative tasks. |
| work_in_progress | txflow.work_in_progress | Unpublished functions which are currently in development. |

