# Transformation Flow Functions

A metholodology and framework is a great starting point, but in order to move from theory to practice we need the practical function set which empowers SQL developers to start realising the benefits of this framework.  These functions are all open-source and readily available to any authenticated BigQuery user in the public `txflow` project[^1].

## `txflow` functions
The functions are organised into function sets around the different phases in a data pipeline:

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
```sql
CALL txflow.function_set.function(argument_1, ... argument_n)
```
However, transformation flow functions will always have the same initial two arguments, with additional subsequent arguments specific to the function. These initial arguments will be:

argument_name | argument_description
--- | :--
`sql_block_name` | The name by which the transformation flow stage will be referenced, which needs to be unique within a transformation flow
`sql_blocks` | The variable which stores all transformation flow stages, which is updated sequentially by each transformation flow function

As such at any point in a transformation flow, the `sql_blocks` variable will be composed of the name, SQL code and dependencies for the current stage and each preceding stage, in order of definition (which is therefore the order of execution).

The exact implementation is explained in the following section.

### BigQuery Variables
Variables in BigQuery _all_ need to be defined at the start of a script, and can be initialised at point of declaration (using the `DEFAULT` keyword) or left uninitalised.  However the precise data type (or data structure and types for more complex variables) _need_ to be be defined at the point of variable declaration.  For example, to declare an uninitialised string variable at the start of a script, the following syntax is used:
```sql
DECLARE my_variable STRING;
```
To initialise at point of declaration the syntax would be:
```sql
DECLARE my_variable STRING DEFAULT "string_value";
```

#### The `sqlblocks` Variable
This variable represents the sequence of transformation flow stages, with one row per transformation flow stage and three variables per row:

variable_name | variable_data_type | variable_description
--- | --- | :--
`sql_blocks.name` | `STRING` | Name of the flow stage (unique within flow)
`sql_blocks.code` | `STRING` | SQL code which defines flow stage
`sql_blocks.dependencies` | `ARRAY<STRING>` | An array of table references or sql_block names which are used in the flow stage.

In BigQuery this variable type is an array of structs, which is the equivalent in Python of an array of dictionaries.  In BigQuery this variable needs to be declared with the precise structure which will contain the transformation flow definition, which means that the following variable needs to be declared at the start of any script:

```sql
DECLARE sql_blocks ARRAY<STRUCT<name STRING, code STRING, dependencies ARRAY<STRING>>>
```

Each time a transformation flow stage is added to the flow, another row is appended to the array.

## Functions vs. Queries
Functions are developed to be shortcuts, where hand-crafting SQL would be unnecessarily verbose and could potentially introduce human error.  A good rule of thumb is:

> If 




[^1]: `txflow` is shorthand for `transformationflow`