# Overview

## What are transformation flow functions?

Transformation flow functions are primarily a combination of Stored Procedures (Routines) and SQL User Defined Functions (UDFs), however JavaScript UDFs are also implemented in some instances to leverage custom-packaged external libraries to extend core BigQuery functionality for some advanced use-cases.

Functions are grouped into core `txflow` functions which are higher-level functions organised by flow stage, and lower-level `txflowutil` functions, which are the building blocks for the higher-order `txflow` functions and are organised by functional area.  These can also be composed into custom functions to fit precise use-cases, but most end-users should only need `txflow` functions to build, analyse and manage data transformations in BigQuery.

## Working with `txflow` functions
`txflow` functions can be used for inspection purposes, to create new views or tables or to return a variable which can then be processed or passed onto additional functions, depending on the specific use-case.  The behaviour of a specific function is defined by the naming suffixes, which can be one of either:

suffix | behaviour | response
--- | --- | ---
_`none`_ | Core function | Query stage results in the results window
`_sql` | Generates the SQL query which defines the function | Returns a SQL query string in an OUT argument
`_view` | Creates or replaces view from the SQL string | New or updated view at `destination_view_ref` from the SQL
`_table` | Creates or replaces table from the SQL string | Creates a new table at `destination_table_ref` from the SQL execution
`_result` | Returns the query execution result | Returns an array of structs as documented in the function definition

---

The precise behaviour is outlined below for the `txflow.profile.column_profile` txflow function family, which generates useful column-level statistics for a single view or table:

function_name_suffix | function_name | function_action 
 --- | --- | --- 
_`none`_ | `column_profile` | Returns the column profile as the final stage in the results window, in addition to previous stages (for inspection or saving/exporting results).
`_sql` | `column_profile_sql` | Returns the column profile SQL query as the OUT `final_query_string` STRING parameter.
`_view` | `column_profile_view` | Builds a view with the column profile at time of creation.  Columns included in the view will be static until re-generated, but data will be dynamic.
`_table` | `column_profile_table` | Builds a table with the column profile and data at time of creation.  Columns and data included in the table will be static until re-generated.
`_result` | `column_profile_result` | Returns an array of structs in the OUT `column_profile` argument.


See the worked examples outlining how to work with the functions with `_query` and `_results` suffixes as they will require an output variable to be declared before the function is called.


## Function Group Definition
Txflow functions are grouped by the stage of the transformation flow that they are expected to be used.

function_group_name | function_group_ref | function_group_description
--- | --- | --- 
`ingest` | `txflow.ingest` | Data Ingestion 
`profile` | `txflow.profile` | Data Profiling
`prepare` | `txflow.prepare` | Data Preparation
`transform` | `txflow.transform` | Data Transformation 
`export` | `txflow.export` | Data Exporting 
`monitor` | `txflow.monitor` | Data Monitoring 