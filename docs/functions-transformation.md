# Data Transformation Functions
These functions form the core of the framework and aim to automate and simplify common data transformation patterns. 

## De-Duplication
Robust de-duplication is typically executed by leveraging a row hash function, as unique identifiers are not enforced in BigQuery.

### `txflow.transform.remove_duplicates`
function_group | function_name | function_output | description
 --- | --- | --- |---
`transform` | `remove_duplicates` | e.g. VIEW | Creates a view with duplicates removed.  Typically follows addition of a rowhash based on a specified subset of columns (`txflow.prepare.add_rowhash`) 

#### Call Syntax
``` sql
CALL txflow.transform.remove_duplicates(
     'source_ref__STRING', 
     'destination_ref__STRING', 
     'partition_by_columns__ARRAY<STRING>', 
     'order_by_columns_with_optional_sort__ARRAY<STRING>'
)
```
#### Arguments
argument | datatype | description
 --- | :-: | ---
`source_ref` | `STRING` | Input table or view reference 
`destination_ref` | `STRING` | New view reference
`partition_by` | `ARRAY <STRING>` | Partition by column(s)
`order_by_columns_with_optional_sort` | `ARRAY <STRING>` | Sort column(s) with optional ASC/DESC (default ASC if omitted)

#### Example
``` sql
CALL txflow.function_group.function_name(
     'project.dataset.input_table', 
     'project.dataset.output_table',
     ['rowhash'], 
     ['timestamp DESC', 'additional_sort_column ASC']
)
```

## JSON Extraction
BigQuery can extract data from JSON stored in STRING fields and restructure it into scalar or array columns using [JSON functions in Standard SQL
](https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions).  

However, extraction of the data requires manual inspection of the exact structure and manually entering the exact path for each element, in addition to understanding whether it is a scalar (single) value or array of elements.

These utilities automate the inspection and enable users to automatically and systematically extract JSON into native BigQuery data structures .

### `txflow.transform.decode_json`
function_group | function_name | function_output | description
 --- | --- | --- |---
`transform` | `decode_json` | VIEW | Decodes the top level of a JSON object, automatically detecting the structure and applying the correct JSON extraction function to each element.

Note that the schema used to generate the decoder SQL is detected from a single JSON object.

#### Call Syntax
``` sql
CALL txflow.transform.decode_json(
     'source_ref__STRING', 
     'destination_ref__STRING', 
     'json_column__STRING', 
     'order_by__STRING'
     )
```
#### Arguments
argument | datatype | description
 --- | :-: | ---
`source_ref` | `STRING` | Source table or view reference 
`destination_ref` | `STRING` | Reference for new output view
`json_column` | `STRING` | Column containing JSON string
`order_by` | `STRING` | 'Order by' column used to determine which object is used for schema detection

## Array Manipulation
The syntax for working with arrays in BigQuery can be difficult to master (see [Array functions in Standard SQL
](https://cloud.google.com/bigquery/docs/reference/standard-sql/array_functions)), despite the comprehensive documentation and examples (e.g. [Working with arrays in Standard SQL
](https://cloud.google.com/bigquery/docs/reference/standard-sql/arrays)).

These functions automate SQL generation for typical array manipulation patterns to simplify and accelerate the transformation development process.


### `txflow.transform.unnest_array`
function_group | function_name | function_output | description
 --- | --- | --- |---
`transform` | `unnest_array` | VIEW | Unnests a single array field and flattens the output into a standard view, with optional exclusions of columns which should not be repeated in the output view.

#### Call Syntax
```
CALL txflow.transform.unnest_array(
    'source_ref__STRING', 
    'destination_ref__STRING', 
    'unnest_array_column__STRING', 
    ['exclude_columns__ARRAY_STRING']
    )
```

#### Arguments
argument | datatype | description
 --- | :-: | ---
`source_ref` | `STRING` | Source table or view
`destination_ref` | `STRING` | Reference of new output view
`unnest_array_column` | `STRING` | Column containing array to be unnested
`exclude_columns` | `ARRAY<STRING>` | Columns to exclude from output table. To include all columns you must set this as an empty array ('[]')


