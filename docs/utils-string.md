# String Utility Functions `(txflowutils.string)`
These functions encapsulate existing low-level actions and add additional functionality for common string analysis and processing functions.
 
### `string.compute_levenshtein_distance`
function_group | function_name | function_output | description
 --- | --- | --- |---
`string` | `compute_levenshtein_distance` | INT64 | Computes the Levenshtein Distance between two input strings

The [Levenshtein Distance](https://en.wikipedia.org/wiki/Levenshtein_distance) is the minimum number of single character edits (insertions, deletions or substitutions) required to change one string into another string.  It can be used to e.g. match data on human-entered string fields.

#### Call Syntax
```
CALL txflow.string.compute_levenshein_distance(
     'string_x__STRING', 
     'string_y__STRING'
     )
```
#### Arguments
argument | datatype | description
 --- | :-: | ---
`string_x` | `STRING` | First string to compare
`string_y` | `STRING` | Second string to compare

