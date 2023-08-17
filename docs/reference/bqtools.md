---
hide:
  - navigation
---

# Summary
Product | Project ID | Access | Description
-- | -- | -- | --
BigQuery Tools | `bqtools` | Private | The low-level building blocks used to build, deploy and manage data automation solutions, capabilities and products.


# Deployment
Functions are currently deployed into the following [BigQuery regions](https://cloud.google.com/bigquery/docs/locations):

Region Name | Dataset ID 
--- | --- 
EU | `bqtools.eu` 
US | `bqtools.us` 


# Functions
## `get_sql`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`get_sql` | Returns the SQL definition of a single view or routine | PROCEDURE | routine_or_view_id STRING | sql STRING | `bqtools-qb..get_sql`

??? info "example: `get_sql`"
    === "EU"
        ```sql
        DECLARE sql STRING;
        CALL bqtools.eu.get_sql(routine_or_view_id, sql);
        ```

    === "US"
        ```sql
        DECLARE sql STRING;
        CALL bqtools.us.get_sql(routine_or_view_id, sql);
        ```

## `profile_query_ctes`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`profile_query_ctes` | REMOTE | Returns the JSON profile of a CTE-based query | sql_query STRING | cte_profile_json STRING | `bqtools.cloudfunctions.net/profile-query-ctes`

??? info "example: `profile_query_ctes`"
    === "EU"
        ```sql
        DECLARE cte_profile JSON;
        SET cte_profile = (SELECT PARSE_JSON(bqtools.eu.profile_query_ctes(sql)))
        ```

    === "US"
        ```sql
        DECLARE cte_profile JSON;
        SET cte_profile = (SELECT PARSE_JSON(bqtools.eu.profile_query_ctes(sql)))
        ```

??? question "response: `profile_query_ctes`"
    The response will be a JSON string representation of the `cte_profile`, containing an array of objects (`[{index, name, sql, dependencies},...]`), for example:
    ```json
    [
    {"index": 0, "name": "get_data_a", "sql": "SELECT * FROM `project_id.dataset_name.table_a`", "dependencies": ["project_id.dataset_name.table_a"]},
    {"index": 1, "name": "get_data_b", "sql": "SELECT * FROM `project_id.dataset_name.table_b`", "dependencies": ["project_id.dataset_name.table_b"]},
    {"index": 2, "name": "union_all_data", "sql": "SELECT * FROM get_data_a UNION ALL SELECT * FROM get_data_b", "dependencies": ["get_data_a", "get_data_b"]}
    ]
    ```

    This response can be converted into a JSON object using `PARSE_JSON` as in the example above. 

## `update_cte_sql`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`update_cte_sql` | PROCEDURE | Replaces the SQL for a single CTE (identified by name) in a `cte_profile` object | INOUT cte_profile JSON, update_cte_name STRING, update_cte_sql STRING_ | _INOUT cte_profile JSON | `bqtools..get_cte_profile_index`


## `build_sql_from_cte_profile`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`build_sql_from_cte_profile` | PROCEDURE | Builds a SQL query from a `cte_profile` object | cte_profile JSON, INOUT sql STRING | INOUT sql STRING | 