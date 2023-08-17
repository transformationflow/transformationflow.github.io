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

# Objects
## `cte_profile (JSON)` 
The CTE (Common Table Expression) profile is a JSON representation of the structure of a single cte-structured SQL query.  It contains the following fields:

Field Name | Field Type | Field Description
--- | --- | ---
index | STRING | CTE sequence
name | STRING | CTE name
sql | STRING | CTE SQL definition
dependencies | ARRAY<STRING\> | CTE dependencies by CTE alias or table_id

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
## `get_sql`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`get_sql` | PROCEDURE | Returns the SQL definition of a single view or routine | routine_or_view_id STRING | sql STRING | `bqtools-qb..get_sql`

??? info "execution: `get_sql`"
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

??? info "execution: `profile_query_ctes`"
    === "EU"
        ```sql
        DECLARE cte_profile JSON;
        SET cte_profile = (SELECT PARSE_JSON(bqtools.eu.profile_query_ctes(sql)));
        ```

    === "US"
        ```sql
        DECLARE cte_profile JSON;
        SET cte_profile = (SELECT PARSE_JSON(bqtools.us.profile_query_ctes(sql)));
        ```

## `update_cte_sql`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`update_cte_sql` | PROCEDURE | Replaces the SQL for a single CTE (identified by name) in a `cte_profile` object | INOUT cte_profile JSON, update_cte_name STRING, update_cte_sql STRING | INOUT cte_profile JSON | `bqtools..get_cte_profile_index`

??? info "execution: `update_cte_sql`"
    === "EU"
        ```sql
        CALL bqtools.eu.update_cte_sql(cte_profile, update_cte_name, update_cte_sql);
        ```

    === "US"
        ```sql
        CALL bqtools.us.update_cte_sql(cte_profile, update_cte_name, update_cte_sql);
        ```

## `build_sql_from_cte_profile`
Function Name | Function Type | Description | Arguments | Returns | Dependencies
--- | --- | --- | --- | --- | ---
`build_sql_from_cte_profile` | PROCEDURE | Builds a SQL query from a `cte_profile` object | cte_profile JSON, INOUT sql STRING | INOUT sql STRING | 

??? info "execution: `build_sql_from_cte_profile`"
    === "EU"
        ```sql
        DECLARE sql STRING;
        CALL bqtools.eu.build_sql_from_cte_profile(cte_profile, sql);
        ```

    === "US"
        ```sql
        DECLARE sql STRING;
        CALL bqtools.us.build_sql_from_cte_profile(cte_profile, sql);
        ```