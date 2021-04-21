# References

## Transformation Flow Stage Codes
Transformation stages are suffixed with two letter codes following a double underscore to denote the type of transformation which is taking place.  Multiple transformation types in one view will have the codes appended, whereas transformations in sequential views will have the codes delimited by an underscore.  For example, row hashing and de-duplicating `inbound_data` in one view would result in the view name `inbound_data__rhdd`, whereas executing the transformation in two stages would result in views `inbound_data__rh` and `inbound_data__rh_dd` appearing in sequence.

Codes are as follows:

action_code | action_name | action_description
--- | --- | ---
`cf` | Column Filter | Filter out columns
`rf` | Row Filter | Filter out rows
`dt` | Data Types | Change data types
`nu` | Numeric | Convert columns to number types
`cr` | Column Rename | Rename columns
`cl` | Clean | Clean data
`pv` | Pivot | Pivot data into additional columns
`up` | Unpivot | Unpivot data into key-value pairs
`ag` | Array Aggregate | Aggregate into arrays
`xo` | Extract Object | Extract JSON objects
`xa` | Extract Array | Extract JSON arrays
`fl` | Flatten | Flatten arrays
`rh` | Row Hash | Build row hash identifier
`dd` | De-Duplicate | Remove duplicate rows
`lk` | Lookup | Lookup data from additional source
`md` | Metadata | Join in metadata
`jo` | Join | Join in additional data

