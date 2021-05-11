# Data Ingestion Functions Overview

## External Tables Overview
Add stuff here and link to Medium article


## Simple Ingestion
Simplification of external table creation from Google Cloud Storage (GCS) buckets and Google Sheets (GS), enabling users to create new external tables directly in the query editor.

### `ingest.csv_from_gcs`
### `ingest.from_gs`

## Safe Ingestion
Automatic external table creation from Google Cloud Storage (GCS) buckets and Google Sheets (GS), setting all data types to strings for ingest robustness.  Note that these columns will require subsequent data type correction.

### `ingest.csv_from_gcs_safe`

### `ingest.from_gcs_safe`


## Streaming Ingestion
Streaming inserts are a robust and performant mechanism for ingesting data into BigQuery. 


### `ingest.create_or_replace_streaming_table`

