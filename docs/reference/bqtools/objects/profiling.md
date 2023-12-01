# **`cte_profile (JSON)`**
The CTE (Common Table Expression) profile (`cte_profile`) is a `JSON` representation of the structure of a single cte-structured SQL query.  It contains the following fields:

Field Name | Field Type | Field Description
--- | --- | ---
`index` | `STRING` | CTE order of sequence 
`name`| `STRING` | CTE alias
`sql` | `STRING` | CTE SQL definition
`dependencies` | `ARRAY<STRING>` | CTE dependencies by alias or `table_id`

??? abstract "example: `cte_profile`"
    The `cte_profile` for the following SQL query:
    ```
    WITH
    get_data_a AS (
      SELECT * 
      FROM `project_id.dataset_name.table_a`
    ),

    get_data_b AS (
      SELECT * 
      FROM `project_id.dataset_name.table_b`
    ),

    union_all_data AS (
      SELECT * FROM get_data_a UNION ALL 
      SELECT * FROM get_data_b
    )


    SELECT * 
    FROM union_all_data
    ```

    Will be the following JSON string:
    
    ```json
    [
    {"index": 0, "name": "get_data_a", "sql": "SELECT * FROM `project_id.dataset_name.table_a`", "dependencies": ["project_id.dataset_name.table_a"]},
    {"index": 1, "name": "get_data_b", "sql": "SELECT * FROM `project_id.dataset_name.table_b`", "dependencies": ["project_id.dataset_name.table_b"]},
    {"index": 2, "name": "union_all_data", "sql": "SELECT * FROM get_data_a UNION ALL SELECT * FROM get_data_b", "dependencies": ["get_data_a", "get_data_b"]}
    ]
    ```

    This JSON string response can be converted into a `cte_profile` `JSON` object using the `PARSE_JSON` function on the response. Note that any SQL outside of the CTEs will not be included in the `cte_profile`.
