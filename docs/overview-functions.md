# Transformation Flow Functions

A metholodology and framework is a great starting point, but in order to move from theory to practice we need the practical function set which empowers us as SQL developers/authors to start realising the benefits of this framework.  These functions are all open-source and readily available to any authenticated BigQuery user in the public `txflow`[^1]project.

## `txflow` functions
These functions are organised in sets around the different steps in a data pipeline:

- `txflow.ingest`- functions which support setup of data ingestion points into BigQuery.
- `txflow.profile`- functions which support profiling of data at every point in the transformation flow.
- `txflow.transform`- the core functions which support all forms of data transformation actions.
- `txflow.export`- functions which support post-transformation data export tasks.
- `txflow.monitor`- functions which support monitoring of data ingestion, transformation, export and usage.
- `txflow.admin`- functions which support automation of common general administrative tasks.

## Function Usage
In order to understand how to use txflow functions in a transformation flow, it is important to understand a few things about variables and scripting in BigQuery.

### Calling Functions
The function call syntax in BigQuery always follows the same pattern:
```
CALL txflow.function_set.function(argument_1, ... argument_n)
```
However, transformation flow functions will always have the same initial two arguments, with additional subsequent arguments specific to the function. The first argument will always be the `sql_block_name`, which will be the name by which the transformation flow stage will be referenced,  Each `sql_block_name` needs to be unique within a transformation flow 





[^1]: `txflow` is shorthand for `transformationflow`