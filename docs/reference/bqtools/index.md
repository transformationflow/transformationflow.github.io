The **BigQuery Tools** (`bqtools`) library simplifies atomic data operations in BigQuery and enables functional programming and automation in the native BigQuery environment.

# Summary
_Attribute_ | Value
--- | ---
_**Name**_ | BigQuery Tools
_**Project ID**_ | `bqtools`
_**Access**_ | Private & Client
_**Description**_ | The low-level building blocks used to build, deploy and manage data automation solutions and products. It enables functional engineering within the native BigQuery environment. 

# Deployment
Functions are currently deployed into the following [BigQuery regions](https://cloud.google.com/bigquery/docs/locations), but can be mirrored to additional regions as required:

Region Name | Dataset ID 
--- | --- 
EU | `bqtools.eu` 
US | `bqtools.us` 

# Functions
## Resource Creation

- `create_function`
- `create_procedure`
- `create_table`
- `create_table_from_date_bounded_table_function`
- `create_table_function`
- `create_view`
