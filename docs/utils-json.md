# JSON Utility Functions `(txflowutils.json)`
These functions extend BigQuery functionality by enabling automatic schema recognition and associated utilities.

## High Level Utility Functions
These functions can be used to get well-structured responses which are designed to be human-readable, informative and easy to consume as inputs to other functions.

### `json.get_node_schema`
Analysis of a JSON blob and returns a node summary with one row for each node including path, depth, structure (array/value) and type (string, number, object).

## Low-level Utility Functions
These functions form the building blocks of higher-level `txflowutil` and `txflow` functions, and are not intended to be called directly by users.  Responses may require restructuring and/or cleaning to render them useable or intelligible. 

### `json.get_schema`
Takes a JSON object or array string as an input and outputs the schema as a JSON string.

### `json.get_schema_from_array`
Takes an array of JSON objects or array strings as an input and outputs the schema as a JSON string.

### `json.flatten_object`
Takes a JSON string as an input and flattens all nodes into a single column.



