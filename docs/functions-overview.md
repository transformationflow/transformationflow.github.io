# Overview

Functions are primarily a combination of Stored Procedures (Routines) and SQL User Defined Functions.  JavaScript User Defined functions are also implemented in some instances, which leverage custom-packaged external libraries to extend core BigQuery functionality.

Functions are grouped into core `txflow` functions which are higher-level functions organised by flow stage, and lower-level `txflowutil` functions, which are the building blocks for the higher-order `txflow` functions and are organised by functional area.  These can also be composed into custom functions to fit precise use-cases.

Function naming is based on an [action]_[detail]_[optional_suffix] structure, and the behaviour of the function depends on the optional suffix:

suffix | example | behaviour
--- | --- | ---
 none | `txflowutils.arrays.compare_string_arrays` | Results are visible in the results window only (for inspection purposes)
_out | `txflowutils.arrays.compare_string_arrays_out` | Results will be returned by the function into a variable (which must be declared and passed as the final argument)
_out_view | `txflowutils.arrays.compare_string_arrays_out_view` | Output will result in a new view, with the view ref as the final argument
_out_table | `txflowutils.arrays.compare_string_arrays_out_table` | Output will result in a new table, with the table ref as the final argument










Functions will either:

- return a scalar value which can be used in a SQL query
- return data in a variable which can be used in a script to pass data between functions
- create a new view or table 
- display the outcome in the Query results window (for inspection only)

## Function Group Definition
function_group_name | function_group_ref | function_group_description
--- | --- | --- 
`ingest` | `txflow.ingest` | Data Ingestion 
`profile` | `txflow.profile` | Data Profiling
`prepare` | `txflow.prepare` | Data Preparation
`transform` | `txflow.transform` | Data Transformation 
`export` | `txflow.export` | Data Exporting 
`monitor` | `txflow.monitor` | Data Monitoring 


