# References

## Transformation Flow Stage Codes
Transformation stages are suffixed with two letter codes following a double underscore to denote the type of transformation which is taking place.  Multiple transformation types in one view will have the codes appended, whereas transformations in sequential views will have the codes delimited by an underscore.  For example, row hashing and de-duplicating `inbound_data` in one view would result in the view name `inbound_data__rhdd`, whereas executing the transformation in two stages would result in views `inbound_data__rh` and `inbound_data__rh_dd` appearing in sequence.

Codes are as follows:

action_code | action_command | action_name | action_description
--- | --- | --- | ---
`cf` | COLUMN_FILTER |  Column Filter | Filter out columns
`rf` | ROW_FILTER |  Row Filter | Filter out rows
`dt` | DATA_TYPES |  Data Types | Change data types
`nu` | NUMERIC |  Numeric | Convert columns to number types
`cr` | COLUMN_RENAME |  Column Rename | Rename columns
`cl` | CLEAN |  Clean | Clean data
`pv` | PIVOT |  Pivot | Pivot data into additional columns
`up` | UNPIVOT |  Unpivot | Unpivot data into key-value pairs
`ag` | ARRAY_AGGREGATE |  Array Aggregate | Aggregate into arrays
`dj` | DECODE_JSON |  Decode JSON | Decode JSON object
`xo` | EXTRACT_JSON_OBJECT |  Extract Object | Extract JSON objects
`xa` | EXTRACT_ARRAY|  Extract Array | Extract JSON arrays
`fl` | FLATTEN_ARRAY |  Flatten | Flatten arrays
`rh` | ROW_HASH |  Row Hash | Build row hash identifier
`dd` | DEDUPLICATE |  De-Duplicate | Remove duplicate rows
`lk` | LOOKUP |  Lookup | Lookup data from additional source
`md` | METADATA |  Metadata | Join in metadata
`jo` | JOIN |  Join | Join in additional data

