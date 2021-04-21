# References

## Transformation Flow Action Codes
Transformation stages are suffixed with two letter codes following a double underscore to denote the type of transformation action which is taking place.  

Multiple transformation actions in one view will have the codes appended, whereas transformation actions in sequential views will have the codes delimited by an underscore.  

!!! abstract "Example: Action Codes in View Name Suffixes" 
    Row hashing and de-duplicating `inbound_data` in one view will result in the view:

    * `inbound_data__rhdd`
    
    whereas executing the transformation in two stages will result in two views (appearing in sequence):
    
    * `inbound_data__rh`
    * `inbound_data__rh_dd`

    This is important to note as tables and views will always be dispayed in ascending alphabetical order, so this naming strategy groups sequential transformation stages in a logically coherent manner.

### Action Codes

action_code | action_command | action_description
--- | --- | --- 
`cf` | `COLUMN_FILTER` | Filter out columns
`rf` | `ROW_FILTER` | Filter out rows
`dt` | `DATA_TYPES` | Change data types
`nu` | `NUMERIC` | Convert columns to number types
`cr` | `COLUMN_RENAME` | Rename columns
`cl` | `CLEAN` | Clean data
`pv` | `PIVOT` | Pivot data into one column per metric
`up` | `UNPIVOT` | Unpivot data into key-value pairs
`ag` | `ARRAY_AGGREGATE` | Aggregate into array(s)
`dj` | `DECODE_JSON` | Decode JSON object(s)
`xo` | `EXTRACT_JSON_OBJECT` | Extract JSON object(s)
`xa` | `EXTRACT_ARRAY`| Extract JSON array(s)
`fl` | `FLATTEN_ARRAY` | Flatten array(s)
`rh` | `ROW_HASH` | Build row hash identifiers
`dd` | `DEDUPLICATE` | Remove duplicate rows
`lk` | `LOOKUP` | Lookup data from additional source(s)
`md` | `METADATA` | Join in metadata
`jo` | `JOIN` | Join in additional data
`cm` | `COMPUTE` | Compute metric(s)

