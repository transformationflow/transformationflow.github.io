These functions are used to profile SQL queries, and objects are used to store the profile in `JSON` format. 

The profiler uses [SQLGlot](https://sqlglot.com/) to parse GoogleSQL code.

# **`profile_query_ctes`**
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

# **`decode_cte_profile_external_dependencies`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `decode_cte_profile_external_dependencies`
_**ID**_ | `bqtools.[region].decode_cte_profile_external_dependencies`
_**Description**_ | Extracts the external dependency `resource_ids` from a `cte_profile` object relating to a single `resource_id`.
_**Type**_ | `FUNCTION (SQL)`
_**Arguments**_ | `resource_id STRING, cte_profile JSON`
_**Returns**_ | `ARRAY<dependencies STRUCT<resource_id STRING, resource_dependency STRING>`
_**Dependencies**_ | `bqtools.[region].decode_cte_profile_external_dependencies`

!!! info "execution: `decode_cte_profile_external_dependencies`"
    === "EU"
        ```sql
        SELECT  bqtools.eu.decode_cte_profile_external_dependencies(resource_id, cte_profile);
        ```

    === "US"
        ```sql
        SELECT  bqtools.us.decode_cte_profile_external_dependencies(resource_id, cte_profile);
        ```

??? abstract "example: `decode_cte_profile_external_dependencies`"
    In this example, the `cte_profile` objects are stored in the `profile_value JSON` column in the `BQMANAGER.PROFILE` table. This query will result in a unnested set of dependency pairs which can then be used for lineage profiling and mapping.

    === "EU"
        ```sql
        SELECT 
        dependency
        FROM `[project_id].BQMANAGER.PROFILE`
        LEFT JOIN 
        UNNEST(bqtools.eu.decode_cte_profile_external_dependencies(resource_id, profile_value).dependencies) AS dependency
        ```

    === "US"
        ```sql
        SELECT 
        dependency
        FROM `[project_id].BQMANAGER.PROFILE`
        LEFT JOIN 
        UNNEST(bqtools.us.decode_cte_profile_external_dependencies(resource_id, profile_value).dependencies) AS dependency
        ```
    