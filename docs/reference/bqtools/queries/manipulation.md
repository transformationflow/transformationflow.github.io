These functions are used to manipulate and build SQL queries.  They enable robust arbitrary manipulation and evaluation of SQL queries in the native BigQuery environment.

# **`update_cte_sql`**
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

# **`build_query_from_cte_profile`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `build_query_from_cte_profile`
_**ID**_ | `bqtools.[region].build_query_from_cte_profile`
_**Description**_ | Builds a CTE-structured SQL query from a `cte_profile` object.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `cte_profile JSON, OUT sql STRING`
_**Returns**_ | `OUT sql STRING`
_**Dependencies**_ | `bqtools.[region].get_cte_profile_index`

!!! info "execution: `build_query_from_cte_profile`"
    === "EU"
        ```sql
        CALL `bqtools-qb.eu.build_query_from_cte_profile`(cte_profile, sql);
        ```

    === "US"
        ```sql
        CALL `bqtools-qb.us.build_query_from_cte_profile`(cte_profile, sql);
        ```

# **`build_struct_query_from_cte_profile`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `build_struct_query_from_cte_profile`
_**ID**_ | `bqtools.[region].build_struct_query_from_cte_profile`
_**Description**_ | Builds a CTE-structured SQL query from a `cte_profile` object, using a `SELECT AS STRUCT *` as the final select query.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `cte_profile JSON, OUT sql STRING`
_**Returns**_ | `OUT sql STRING`
_**Dependencies**_ | `bqtools.[region].get_cte_profile_index`

!!! info "execution: `build_struct_query_from_cte_profile`"
    === "EU"
        ```sql
        CALL `bqtools-qb.eu.build_struct_query_from_cte_profile`(cte_profile, sql);
        ```

    === "US"
        ```sql
        CALL `bqtools-qb.us.build_struct_query_from_cte_profile`(cte_profile, sql);
        ```