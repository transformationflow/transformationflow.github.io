These objects and functions are used to profile, manipulate and build SQL queries.  They enable robust arbitrary manipulation and evaluation of SQL queries in the native BigQuery environment.

The profiler uses [SQLGlot](https://sqlglot.com/) to parse GoogleSQL code.

# Objects
## **`cte_profile (JSON)`**
The CTE (Common Table Expression) profile (`cte_profile`) is a JSON representation of the structure of a single cte-structured SQL query.  It contains the following fields:

Field Name | Field Type | Field Description
--- | --- | ---
`index` | `STRING` | Sequential order 
`name`| `STRING` | Alias
`sql` | `STRING` | SQL definition
`dependencies` | `ARRAY<STRING>` | Dependencies by alias or `table_id`

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

    This JSON string response can be converted into a JSON object using the `PARSE_JSON` function on the response. Note that any SQL outside of the CTEs will not be included in the `cte_profile`.

# Functions

## **`profile_query_ctes`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `profile_query_ctes`
_**ID**_ | `bqtools.[region].profile_query_ctes`
_**Description**_ | Returns the JSON profile of a CTE-based query
_**Function Type**_ | `REMOTE`
_**Arguments**_ | `sql_query STRING`
_**Returns**_ | `cte_profile_json STRING`
_**Dependencies**_ | `bqtools.cloudfunctions.net/profile-query-ctes`

!!! info "execution: `profile_query_ctes`"
    === "EU"
        ```sql
        SET cte_profile = (SELECT PARSE_JSON(bqtools.eu.profile_query_ctes(sql)));
        ```

    === "US"
        ```sql
        SET cte_profile = (SELECT PARSE_JSON(bqtools.us.profile_query_ctes(sql)));
        ```

## **`update_cte_sql`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `update_cte_sql`
_**ID**_ | `bqtools.[region].update_cte_sql`
_**Description**_ | Replaces the SQL for a single CTE (identified by name) in a `cte_profile` object
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `INOUT cte_profile JSON, update_cte_name STRING, update_cte_sql STRING`
_**Returns**_ | `INOUT cte_profile JSON`
_**Dependencies**_ | `bqtools.[region].get_cte_profile_index`

!!! info "execution: `update_cte_sql`"
    === "EU"
        ```sql
        CALL bqtools.eu.update_cte_sql(cte_profile, update_cte_name, update_cte_sql);
        ```

    === "US"
        ```sql
        CALL bqtools.us.update_cte_sql(cte_profile, update_cte_name, update_cte_sql);
        ```

## **`build_sql_from_cte_profile`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `build_sql_from_cte_profile`
_**ID**_ | `bqtools.[region].build_sql_from_cte_profile`
_**Description**_ | Builds a SQL query from a `cte_profile` object
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `cte_profile JSON, OUT sql STRING`
_**Returns**_ | `OUT sql STRING`
_**Dependencies**_ | `bqtools.[region].get_cte_profile_index`

!!! info "execution: `build_sql_from_cte_profile`"
    === "EU"
        ```sql
        CALL `bqtools-qb.eu.build_sql_from_cte_profile`(cte_profile, sql);
        ```

    === "US"
        ```sql
        CALL `bqtools-qb.us.build_sql_from_cte_profile`(cte_profile, sql);
        ```