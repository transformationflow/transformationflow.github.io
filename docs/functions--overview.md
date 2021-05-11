# Overview

Functions are primarily a combination of Stored Procedures (also interchangeably referred to as Routines) and SQL User Defined Functions.  

JavaScript User Defined functions are also implemented in some instances, which leverage custom-packaged external libraries to extend core BigQuery functionality.

Functions are grouped into core `txflow` functions (which are intended to be directly called by users), and lower-level `txflowutil` functions, which are the building blocks for the higher-order `txflow` functions.  These can also be composed into custom functions to fit the precise use-case.

Functions will either:
- return a value which can be used in a SQL query
- return data in a variable which can be used in a script to pass data between functions
- create a new view or table 
- display the outcome in the Query results window (for inspection only)

## Function Group Definition
function_group_name | function_group_ref | function_group_description
--- | --- | --- 
`ingest` | `txflow.ingest` | Data ingestion 
`profile` | `txflow.profile` | Data profiling
`transform` | `txflow.transform` | Data transformation 


