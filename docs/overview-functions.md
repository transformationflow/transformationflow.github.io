# Transformation Flow Functions

A metholodology and framework is a great starting point, but in order to move from theory to practice we need the practical function set which empowers SQL developers to start realising the benefits of this framework.  These functions are all open-source and readily available to any authenticated BigQuery user in the public `txflow` project[^1].

## `txflow` functions
The functions are organised into function sets around the different steps in a data pipeline:

function_set | function_set_ref | function_set_description
--- | --- | :-- 
`ingest` | `txflow.ingest` | Functions which support setup of data ingestion points into BigQuery
`profile` | `txflow.profile` | Functions which support profiling of data at every point in the transformation flow
`transform` | `txflow.transform` | Core functions which support all forms of data transformation actions
`export` | `txflow.export` | Functions which support post-transformation data export tasks
`monitor` | `txflow.monitor` | Functions which support monitoring of data ingestion, transformation, export and usage
`admin` | `txflow.admin` | Functions which support automation of common general administrative tasks


## Function Usage
In order to understand how to use `txflow` functions in a transformation flow, it is important to understand a few nuances relating to variables and scripting in BigQuery.

### Calling Functions
The function call syntax in BigQuery always follows the same pattern:
```
CALL txflow.function_set.function(argument_1, ... argument_n)
```
However, transformation flow functions will always have the same initial two arguments, with additional subsequent arguments specific to the function. These arguments will be:

argument_name | argument_description
--- | :--
`sql_block_name` | The name by which the transformation flow stage will be referenced, which needs to be unique within a transformation flow
`sql_blocks` | The variable which stores all transformation flow stages, which is updated sequentially by each transformation flow function

As such at any point in a transformation flow, the `sql_blocks` variable will be composed of the name, SQL code and dependencies for the current stage and each preceding stage, in order of definition (which is therefore the order of execution).

The exact implementation is explained in the following section.

#### BigQuery Variables
Variables in BigQuery _all_ need to be defined at the start of a script, and can be initialised or left uninitalised.  However the precise data type (or data structure and types for more complex variables) need to be be defined at the point of variable declaration.


#### The `sqlblocks` Variable








[^1]: `txflow` is shorthand for `transformationflow`