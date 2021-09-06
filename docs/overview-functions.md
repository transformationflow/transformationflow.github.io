# Transformation Flow Functions
The approach is based around a core set of functions, which are all open-source and readily available to any authenticated BigQuery user in the public `txflow` and `txflowutils` projects[^1].

## `txflow` functions
Transformation Flow (`txflow`) functions are organised into logical groups depending on the type of activity they support.  

These groups are either aligned to the sequential stages in a typical data pipeline (`ingest` → `profile` → `transform` → `export` → `monitor`) or groups which support data administration (`admin`) or the development of new functionality and flows (`work_in_progress`, `transformations`).

dataset | dataset_ref | function_set_description
--- | :-- | :-- 
`ingest` | `txflow.ingest` | Functions which support setup of data ingestion points into BigQuery
`profile` | `txflow.profile` | Functions which support profiling of data at every point in the transformation flow
`transform` | `txflow.transform` | Core functions which support all forms of data transformation actions
`export` | `txflow.export` | Functions which support post-transformation data export tasks
`monitor` | `txflow.monitor` | Functions which support monitoring of data ingestion, transformation, export and usage
`admin` | `txflow.admin` | Functions which support automation of common general administrative tasks

## Calling Functions
The function call syntax in BigQuery always follows the same pattern:
```sql
CALL txflow.function_set.function(argument_1, ... argument_n)
```
However, transformation flow functions will always have the same initial two arguments, with additional subsequent arguments specific to the function. These initial arguments will be:

argument_name | argument_description
--- | :--
`sql_block_name` | The name by which the transformation flow stage will be referenced, which needs to be unique within a transformation flow
`sql_blocks` | The array variable which stores all transformation flow stages, which is updated sequentially by each transformation flow function

As such at any point in a transformation flow, the `sql_blocks` array variable will consist of the name of each transformation flow stage, the SQL code to execute the transformation stages, and the dependencies for each stage, in order of definition (and therefore execution).

## Function Outputs
The `sql_blocks` array variable can then be used in a number of different ways by calling `txflow.blocks` functions:

function_ref | function_output
--- | :--
`txflow.blocks.build_sql_blocks_query` | Builds the transformation flow into a single common table expression-based SQL query and returns the **query** as the final result
`txflow.blocks.execute_sql_blocks` | Builds the transformation flow into a single common table expression-based SQL query and executes the resulting query, returning the **query result** as the final result
`txflow.blocks.create_view_from_sql_blocks` | Builds the transformation flow into a single common table expression-based SQL query and **creates a single view** from the resulting query
`txflow.blocks.create_views_from_sql_blocks` | Builds the transformation flow into multiple single queries and **creates a single view for each stage** of the transformation flow

All of these functions also have a `_subset` equivalent function, which will then execute the function and build the output _up to_ a single transformation flow stage.  

This means that with one centrally defined transformation flow it is possible to build any number of output views, which will all be self contained common table expression based views containing the full end-to-end transformation from input to output data.

[^1]: `txflow` is an abbreviation for `transformationflow`