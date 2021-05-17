# Data Visualization Functions `(txflowutils.dataviz)`
These functions enable basic data visualisation within the BigQuery UI.

### `txflowutils.dataviz.build_bar`
function_group | function_name | function_output | description
 --- | --- | --- |---
`dataviz` | `build_bar` | STRING | Returns a string of between 0-100 character for building bar-chart vizualisations.

#### Call Syntax
```
CALL txflow.function_group.function_name(
     'percentage_FLOAT64,  
     'character__STRING'
     )
```

#### Arguments
argument | datatype | description
 --- | :-: | ---
`percentage` | `FLOAT64` | Number between zero and 100 (numbers over 100 will display 100 characters only)
`character` | `STRING` | Character to be printed (e.g. "\|")

