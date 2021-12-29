# Data Preparation Functions
These functions streamline common actions relating to preparation of data, including data type correction and creation of unique ids or row hashes for subsequent transformation.  Data preparation functions will have the same number of output rows as input rows, but may add additional columns.

## Hashing Functions
Hashing functions encode the values in selected columns into a unique identifier, which is essential when e.g. comparing rows of data to identify duplicates.

### `txflow.prepare.add_rowhash`
function_group | function_name | function_output | description
 --- | --- | --- |---
`prepare` | `add_rowhash` | VIEW | Creates a new view with an additional `rowhash` column, which is a hexadecimal MD5 hash of the string concatenation of all columns (excluding those defined in the arguments), controlling for null values.

#### Call Syntax
``` sql
CALL txflow.transform.add_rowhash (
     'source_ref__STRING', 
     'destination_ref__STRING', 
     'column_exclusions__ARRAY_STRING'
)
```
#### Arguments
argument | datatype | description
 --- | :-: | ---
`source_ref` | `STRING` | Source table or view reference 
`destination_ref` | `STRING` | Reference for new output view
`column_exclusions` | `ARRAY <STRING>` | Array of column names to exclude.  To include all then pass an empty array as this argument (`[]`)

#### Example
``` sql
CALL txflow.transform.add_rowhash (
     'project.dataset.input_table', 
     'project.dataset.output_table', 
     ['timestamp_column_to_exclude', 'another_column_to_exclude']
)
```

