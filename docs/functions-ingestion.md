# Data Ingestion Functions Overview
Functions to simplify data ingestion and creation of dynamic ingestion points for BigQuery.

## Safe Ingestion
Automatic external table creation from Google Cloud Storage (GCS) buckets and Google Sheets (GS), setting all data types to strings for ingest robustness.  Note that these columns will require subsequent data type correction.
 
### `txflow.ingest.safe_create_external_table_from_gsheets`
function_group | function_name | function_output | description
 --- | --- | --- |---
`ingest` | `safe_create_external_table_from_gsheets` | EXTERNAL TABLE | Creates an external table which mirrors the content of the first sheet in a Google Sheets workbook

#### Call Syntax
```
CALL txflow.ingest.safe_create_external_table_from_gsheets(
     'destination_ref__STRING', 
     'gsheets_url__STRING'
     )
```
#### Arguments
argument | datatype | description
 --- | :-: | ---
`destination_ref` | `STRING` | Reference of external table to be created
`gsheets_url` | `STRING` | URL of Google Sheet


