---
hide:
  - navigation
---
# Glossary

## BigQuery functions definitions

BigQuery functions used for how.fm transformation

| Function | Arguments | Type | Description | Documentation |
| --- | --- | --- | --- | --- |
| `ARRAY_AGG` | <pre>(</br>&nbsp;&nbsp;&nbsp;[DISTINCT] </br>&nbsp;&nbsp;&nbsp;expression </br>&nbsp;&nbsp;&nbsp;[{IGNORE\|RESPECT} NULLS] </br>&nbsp;&nbsp;&nbsp;[ORDER BY key[{ASC\|DESC}][,... ]] </br>&nbsp;&nbsp;&nbsp;[LIMIT n]</br>) </br>[OVER (...)]</pre>Support all **data types** except `ARRAY`. | **Aggregate function** and returns an `ARRAY` **data type.** | Returns an `ARRAY` of `expression` values. See [Optional Clauses][ARRAY_AGG reference] applied. |[ARRAY_AGG reference] |
| `COUNT` | <pre>(</br>&nbsp;&nbsp;&nbsp;[DISTINCT] </br>&nbsp;&nbsp;&nbsp;expression</br>) </br>[OVER (...)]</pre>The `expression` support any data type. | **Aggregate function** and returns an `INT64` **data type**. | <ol><li>Returns the number of rows in the input.</li></br><li>Returns the number of rows with `expression` evaluated to any value other than `NULL`. </li></br></ol> **Optional Clauses:** </br>`OVER`: Specifies a window. See [Analytic Functions]. </br>`DISTINCT`: Each distinct value of `expression` is aggregated only once into the result. | [COUNT reference] |
| `CURRENT_DATE` | `([time_zone])` | **DATE function** and returns a `DATE` **data type**. | Returns the current date as of the specified or default timezone. Parentheses are optional when called with no arguments. See [Timezone definitions]. | [CURRENT_DATE reference] |
| `DATE` | <ol><li>`DATE(year, month, day)`</li></br><li>`DATE(timestamp_expression[, timezone])`</li></br><li>`DATE(datetime_expression)`</li><ol> | **DATE function** and returns a `DATE` **data type**. | <ol><li>Constructs a `DATE` from `INT64` values representing the year, month, and day. `e.g: DATE(2016, 12, 25) AS date_ymd`</li></br><li>Extracts the `DATE` from a `TIMESTAMP` expression. It supports an optional parameter to [specify a timezone][Timezone definitions]. If no `timezone` is specified, the default `timezone`, `UTC`, is used.. e.g: `DATE(TIMESTAMP "2016-12-25 05:30:00+07", "America/Los_Angeles") AS date_tstz`</li></br><li>Extracts the DATE from a DATETIME expression. e.g: `DATE(DATETIME "2016-12-25 23:59:59") AS date_dt`</li></ol> | [DATE reference] |
| `DATE_DIFF` | `(date_expression_a, date_expression_b, date_part)` | **DATE function** and returns an `INT64` **data type**. | Returns the whole number of specified `date_part` intervals between two `DATE` objects `(date_expression_a - date_expression_b)`. If the first `DATE` is earlier than the second one, the output is negative. | [DATE_DIFF reference] |
| `DATE_TRUNC` | `(date_expression, date_part)` | **DATE function** and returns a `STRING` **data type**. | Truncates the date to the specified granularity. See [DATE_TRUNC reference]. | [DATE_TRUNC reference] |
| `FORMAT_DATE` | `(format_string, date_expr)`| **DATE function** and returns a `STRING` **data type**. | Formats the `date_expr` according to the specified `format_string`. See [Supported Format Elements][SFE] For DATE. | [FORMAT_DATE reference] |
| `EXTRACT` | `(part FROM date_expression)` | **DATE function** and returns an `INT64` **data type**. | Returns the value corresponding to the specified date part. See the [EXTRACT reference] for the specified date part. | [EXTRACT reference] |
| `GENERATE_DATE_ARRAY` | `(start_date, end_date[, INTERVAL INT64_expr date_part])` | **ARRAY function** and returns An `ARRAY` containing 0 or more `DATE` values. | Returns an array of dates. The `start_date` and `end_date` parameters determine the inclusive start and end of the array. <ol><li>`start_date` must be a DATE</li><li>`end_date` must be a DATE</li><li>`INT64_expr` must be an INT64</li><li>`date_part` must be either `DAY, WEEK, MONTH, QUARTER, or YEAR`</li></ol> | [GENERATE_DATE_ARRAY] |
| `INSTR` | `(source_value, search_value[, position[, occurrence]])` | **STRING function** and returns an `INT64` **data type**. | Returns the lowest 1-based index of `search_value` in `source_value`. 0 is returned when no match is found. `source_value` and `search_value` must be the same type, either `STRING` or `BYTES`.</br></br>See [Optional Parameters][INSTR reference] applied. | [INSTR reference] |
| `LAST_VALUE` | `(value_expression [{RESPECT \| IGNORE} NULLS])` |  **NAVIGATION function** and returns the same type as `value_expression`. </br></br>`value_expression` can be any data type that an expression can return. | Returns the value of the `value_expression` for the last row in the current window frame. </br></br>This function includes `NULL` values in the calculation unless `IGNORE NULLS` is present. If `IGNORE NULLS` is present, the function excludes `NULL` values from the calculation. | [LAST_VALUE reference] |
| `LAG` | `(value_expression[, offset [, default_expression]])` | **NAVIGATION function** and returns the same type as `value_expression` | Returns the value of the `value_expression` on a preceding row. <ul><li>`value_expression` can be any data type that can be returned from an expression.</li><li>`offset` must be a non-negative integer literal or parameter</li><li>`default_expression` must be compatible with the value expression type.</li></ul> | [LAG reference]|
| `LOWER` | `(value)` | **STRING function** and returns a `STRING` or `BYTES` | For `STRING` arguments, returns the original string with all alphabetic characters in lowercase.</br></br>For `BYTES` arguments, the argument is treated as ASCII text, with all bytes greater than 127 left intact. | [LOWER reference] |
| `MAX` | `(expression) [OVER (...)]` Support any [orderable] **data type**. | **Aggregate function**. | Returns the maximum value of `non-NULL` expressions. Returns `NULL` if there are zero input rows or expression evaluates to `NULL` for all rows. Returns `NaN` if the input contains a `NaN`. Optional clause: `OVER`: Specifies a window. See [Analytic Functions]. | [MAX reference] |
| `MIN` | `(expression) [OVER (...)]` Support any [orderable] **data type**. | **Aggregate function**. | Returns the minimun value of `non-NULL` expressions. Returns `NULL` if there are zero input rows or expression evaluates to `NULL` for all rows. Returns `NaN` if the input contains a `NaN`. Optional clause: `OVER`: Specifies a window. See [Analytic Functions]. | [MIN reference] |
| `MD5` | `(input)` | **HASH function** and returns `BYTES` _(only 16 bytes)_ **data type**. | Computes the `hash` of the `input` using the [MD5 algorithm][MD5 algo]. The `input` can either be `STRING` or `BYTES`. The `string` version treats the `input` as an `array` of `bytes`. | [MD5 reference] |
| `PARSE_DATE` | `(format_string, date_string)` | **DATE function** and returns a `DATE` **data type**. | Converts a [string representation of date] to a DATE object. | [PARSE_DATE reference] |
| `REGEXP_EXTRACT_ALL` | `(value, regexp)` | **STRING function** and returns an `ARRAY` of either `STRINGs` or `BYTES` | Returns an array of all substrings of `value` that match the regular expression, `regexp`.</br></br>The `REGEXP_EXTRACT_ALL` function only returns non-overlapping matches. For example, using this function to extract `ana` from `banana` returns only one substring, not two. | [REGEXP_EXTRACT_ALL reference] |
| `REPLACE` | `(original_value, from_value, to_value)` | **STRING function** and returns a `STRING` or `BYTES` **data type**. | Replaces all occurrences of `from_value` with `to_value` in `original_value`. If `from_value` is empty, no replacement is made.. | [REPLACE reference] |
| `ROW_NUMBER` | `(empty)` | **Numbering function** and returns an `INT64` **data type**. | Does not require the `ORDER BY` clause. Returns the sequential row ordinal (1-based) of each row for each ordered partition. If the `ORDER BY` clause is unspecified then the result is non-deterministic. | [ROW_NUMBER reference] |
| `SAFE_CAST` | `(expression AS typename [format_clause])` </br>See [format_clause] for `CAST` | **CONVERSION function** | `SAFE_CAST` is identical to `CAST`, except it returns `NULL` instead of raising an error if `BigQuery` is unable to perform the `cast`. | [SAFE_CAST reference] |
| `SAFE_DIVIDE` | `(X, Y)` | **Mathematical function** and returns `INT64`, `FLOAT64`, `NUMERIC`, `BIGNUMERIC` **data type**. | Equivalent to the division operator (`X / Y`), but returns NULL if an error occurs, such as a division by zero error. | [SAFE_DIVIDE reference] |
| `SPLIT` | `(value[, delimiter])` | **STRING function** and returns an `ARRAY` of type `STRING` or `ARRAY` of type `BYTES` | Splits `value` using the `delimiter` argument.</br></br>For `STRING`, the default `delimiter` is the comma `,`.</br></br>For `BYTES`, you must specify a `delimiter`. | [SPLIT reference] |
| `SUBSTR` | `(value, position[, length])` | **STRING function** and returns a `STRING` or `BYTES` **data type**. | Returns a substring of the supplied `STRING` or `BYTES` value. The position argument is an integer specifying the starting `position` of the substring, with position = 1 indicating the first character or byte. The `length` argument is the maximum number of characters for `STRING` arguments, or bytes for `BYTES` arguments. | [SUBSTR reference] |
| `SUBSTRING` | `(value, position[, length])` | **STRING function** and returns a `STRING` **data type**. | Alias for [SUBSTR][SUBSTR reference]. | [SUBSTRING reference] |
| `SUM` | `([DISTINCT] expression) [OVER (...)]` Support any numeric **data types** and `INTERVAL`. | **Aggregate function**. | Returns the sum of non-null values. Optional clauses: `OVER`: Specifies a window. See [Analytic Functions]. `DISTINCT`: Each distinct value of expression is aggregated only once into the result.  | [SUM reference] |
| `TIMESTAMP_DIFF` | `(timestamp_expression_a, timestamp_expression_b, date_part)` | **TIMESTAMP function** and returns a `INT64` **data type** | Returns the whole number of specified `date_part` intervals between two `TIMESTAMP` objects. If the first `TIMESTAMP` is earlier than the second one, the output is negative. </br></br>See [TIMESTAMP_DIFF reference] for supported `values` for `date_part`. | [TIMESTAMP_DIFF reference] |
| `TIMESTAMP_MICROS` | `(int64_expression)` | **TIMESTAMP function** and returns a `TIMESTAMP` **data type**  | Interprets `int64_expression` as the number of microseconds since `1970-01-01 00:00:00 UTC`. | [TIMESTAMP_MICROS reference] |
| `TO_HEX` | `(bytes)` | **STRING function** and returns a `STRING` **data type**. | Converts a sequence of `BYTES` into a hexadecimal `STRING`. To convert a hexadecimal-encoded STRING to BYTES, use [FROM_HEX]. | [TO_HEX reference] |
|`TO_JSON_STRING` | `(value[, pretty_print])` | **JSON function** and returns a `JSON-formatted STRING` **data type** | Takes a `SQL` `value` and returns a `JSON-formatted string` representation of the `value`. Review the supported BigQuery data type `value` and their `JSON encodings` [here][JSON encodings doc]. </br>Optional boolean parameter called `pretty_print`. If `pretty_print` is `true`, the returned `value` is formatted for easy readability. | [TO_JSON_STRING reference] |
| `TRIM` | `(value1[, value2])` | **STRING function** and returns a `STRING` or `BYTES` **data type**. | Removes all leading and trailing characters that match `value2`. If `value2` is not specified, all leading and trailing whitespace characters (as defined by the `Unicode standard`) are removed. If the first argument is of type `BYTES`, the second argument is required. | [TRIM reference] |

## Operators and Statements

| Operator | Type | Description | Documentation |
| --- | --- | --- | --- |
| `DISTINCT`| **SELECT DISTINCT Statement**| A `SELECT DISTINCT` statement discards duplicate rows and returns only the remaining rows. `SELECT DISTINCT` cannot return columns of an `ARRAY` or `STRUCT` types. | [DISTINCT reference] |
| `EXCEPT`| **Set Operator** | The `EXCEPT` operator returns rows from the left input query that are not present in the right input query. | [EXCEPT reference] |
| `LEFT JOIN` | **JOIN operation**  | `LEFT JOIN` indicates that all rows from the left `from_item` are returned; if a given row from the left `from_item` does not join to any row in the right `from_item`, the row will return with NULLs for all columns from the right from_item. </br></br>We recommend to see the [ JOIN types documentation ].| [LEFT JOIN reference] |
| `SAFE_OFFSET`| **Query syntax statement**| <ul><li>`OFFSET`: The index starts at zero. Produces an error if the index is out of range.</li></br><li>`SAFE.prefix`: if you begin a function with the `SAFE.` prefix, it will return `NULL` instead of an error. The SAFE. prefix only prevents errors from the prefixed function itself</li></br><li>`SAFE_OFFSET`: The index starts at zero. Returns `NULL` if the index is out of range.</li></ul> | [OFFSET reference]</br></br>[SAFE prefix reference] |
| `UNNEST`| **UNNEST operator**| The `UNNEST` operator takes an `ARRAY` and returns a table, with one row for each element in the `ARRAY`. You can also use `UNNEST` outside of the `FROM` clause with the [IN operator]. | [UNNEST reference] |

## Conditional expressions

[Conditional expressions] impose constraints on the evaluation order of their inputs. In essence, they are evaluated left to right, with short-circuiting, and only evaluate the output value that was chosen. In contrast, all inputs to regular functions are evaluated before calling the function. Short-circuiting in conditional expressions can be exploited for error handling or performance tuning.

| Function | Arguments | Type | Description | Documentation |
| --- | --- | --- | --- | ---- |
| `CASE` | <pre>CASE</br>&nbsp;&nbsp;&nbsp;WHEN condition THEN result</br>&nbsp;&nbsp;&nbsp;[ ... ]</br>&nbsp;&nbsp;&nbsp;[ ELSE else_result ]</br>END</pre>| **Conditional expression** and returns [Supertype][supertype] of `result`[, ...] and `else_result`.| Evaluates the condition of each successive `WHEN` clause and returns the first result where the condition is true; any remaining `WHEN` clauses and `else_result` are not evaluated. If all conditions are false or NULL, returns `else_result` if present; if not present, returns NULL. </br></br>`condition` must be a boolean expression. </br></br> `result` and `else_result` expressions must be implicitly coercible to a common [supertype]. | [ CASE reference ] |
| `IFNULL` | `(expr, null_result)` | **Conditional expression** and returns [Supertype][supertype] of `expr` or `null_result` **data type.** | If `expr` is `NULL`, return `null_result`. Otherwise, return `expr`. If `expr` is not `NULL`, `null_result` is not evaluated.</br></br> `expr` and `null_result` can be any type and must be implicitly coercible to a common [supertype]. Synonym for `COALESCE(expr, null_result)`. | [IFNULL reference] | |

[//]: # (This are the urls linked above order alphabetically)
[ARRAY_AGG reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions#array_agg
[Analytic Functions]: https://cloud.google.com/bigquery/docs/reference/standard-sql/analytic-function-concepts
[ CASE reference ]: https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions#case
[Conditional expressions]: https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions
[COUNT reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions#count
[CURRENT_DATE reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#current_date
[DATE reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#date
[DATE_DIFF reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#date_diff
[DATE_TRUNC reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#date_trunc
[DISTINCT reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#select_distinct
[EXCEPT reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#except
[EXTRACT reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#extract
[format_clause]: https://cloud.google.com/bigquery/docs/reference/standard-sql/conversion_functions#formatting_syntax
[FORMAT_DATE reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#format_date
[GENERATE_DATE_ARRAY]: https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#generate_date_array
[IFNULL reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions#ifnull
[IN operator]: https://cloud.google.com/bigquery/docs/reference/standard-sql/operators#in_operators
[INSTR reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#instr
[JSON encodings doc]: https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions#json_encodings
[ JOIN types documentation ]: https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#join_types
[LAST_VALUE reference]:https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#last_value
[LAG reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/navigation_functions#lag>
[LOWER reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#lower
[ LEFT JOIN reference ]: https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#left_outer_join
[MAX reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#max>
[MD5 algo]: <https://en.wikipedia.org/wiki/MD5>
[MD5 reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/hash_functions#md5>
[MIN reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#min>
[orderable]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types#orderable_data_types>
[PARSE_DATE reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#parse_date>
[REGEXP_EXTRACT_ALL reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#regexp_extract_all
[REPLACE reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#replace>
[SAFE_CAST reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/conversion_functions#safe_casting>
[SAFE_DIVIDE reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#safe_divide>
[SAFE prefix reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/syntax#safe_prefix
[Set operators]: https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#set_operators
[SFE]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#supported_format_elements_for_date>
[SPLIT reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#split
[string representation of date]:<https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#format_date>
[SUBSTR reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#substr
[SUBSTRING reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#substring
[SUM reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#sum>
[supertype]: https://cloud.google.com/bigquery/docs/reference/standard-sql/conversion_rules#supertypes
[TIMESTAMP_DIFF reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions#timestamp_diff
[TIMESTAMP_MICROS reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions#timestamp_micros>
[FROM_HEX]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#from_hex>
[OFFSET reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#limit_and_offset_clause
[ROW_NUMBER reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#row_number>
[Timezone definitions]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/timestamp_functions#timezone_definitions>
[TO_HEX reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators#to_hex>
[TO_JSON_STRING reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions#to_json_string>
[TRIM reference]: https://cloud.google.com/bigquery/docs/reference/standard-sql/string_functions#trim
[UNNEST reference]: <https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#unnest_operator>
