These functions return metadata related to BigQuery resources.

# Functions
## **`get_dataset_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_dataset_ids`
_**ID**_ | `bqtools.[region].get_dataset_ids`
_**Description**_ | Returns the dataset_ids for all datasets across multiple projects.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `project_ids ARRAY<STRING>, OUT dataset_ids ARRAY<STRING>`
_**Returns**_ | `OUT dataset_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_dataset_ids`

!!! info "execution: `get_dataset_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_dataset_ids(project_ids, dataset_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_dataset_ids(project_ids, dataset_ids);
        ```

## **`get_base_table_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_base_table_ids`
_**ID**_ | `bqtools.[region].get_base_table_ids`
_**Description**_ | Returns an array of ids corresponding to every `BASE TABLE` in a single dataset.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, OUT base_table_ids ARRAY<STRING>`
_**Returns**_ | `OUT base_table_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_base_table_ids`

!!! info "execution: `get_base_table_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_base_table_ids(dataset_id, base_table_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_base_table_ids(dataset_id, base_table_ids);
        ```

## **`get_view_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_view_ids`
_**ID**_ | `bqtools.[region].get_view_ids`
_**Description**_ | Returns an array of ids corresponding to every `VIEW` in a single dataset.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, OUT view_ids ARRAY<STRING>`
_**Returns**_ | `OUT view_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_view_ids`

!!! info "execution: `get_view_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_view_ids(dataset_id, view_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_view_ids(dataset_id, view_ids);
        ```

## **`get_external_table_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_external_table_ids`
_**ID**_ | `bqtools.[region].get_external_table_ids`
_**Description**_ | Returns an array of ids corresponding to every `EXTERNAL TABLE` in a single dataset.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, OUT external_table_ids ARRAY<STRING>`
_**Returns**_ | `OUT external_table_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_external_table_ids`

!!! info "execution: `get_external_table_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_external_table_ids(dataset_id, external_table_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_external_table_ids(dataset_id, external_table_ids);
        ```

## **`get_table_function_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_table_function_ids`
_**ID**_ | `bqtools.[region].get_table_function_ids`
_**Description**_ | Returns an array of ids corresponding to every `TABLE FUNCTION` in a single dataset.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, OUT table_function_ids ARRAY<STRING>`
_**Returns**_ | `OUT table_function_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_table_function_ids`

!!! info "execution: `get_table_function_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_table_function_ids(dataset_id, table_function_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_table_function_ids(dataset_id, table_function_ids);
        ```

## **`get_function_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_function_ids`
_**ID**_ | `bqtools.[region].get_function_ids`
_**Description**_ | Returns an array of ids corresponding to every `FUNCTION` in a single dataset.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, OUT function_ids ARRAY<STRING>`
_**Returns**_ | `OUT function_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_function_ids`

!!! info "execution: `get_function_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_function_ids(dataset_id, function_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_function_ids(dataset_id, function_ids);
        ```

## **`get_procedure_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `get_procedure_ids`
_**ID**_ | `bqtools.[region].get_procedure_ids`
_**Description**_ | Returns an array of ids corresponding to every `PROCEDURE` in a single dataset.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, OUT procedure_ids ARRAY<STRING>`
_**Returns**_ | `OUT procedure_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_procedure_ids`

!!! info "execution: `get_procedure_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_procedure_ids(dataset_id, procedure_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_procedure_ids(dataset_id, procedure_ids);
        ```

## **`get_sql`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_sql`
_**ID**_ | `bqtools.[region].get_sql`
_**Description**_ | Returns the SQL definition of a single `ROUTINE` or `VIEW`.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `routine_or_view_id STRING, INOUT sql STRING`
_**Returns**_ | `OUT sql STRING`
_**Dependencies**_ | `bqtools-qb.[region].get_sql`

!!! info "execution: `get_sql`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_sql(routine_or_view_id, sql);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_sql(routine_or_view_id, sql);
        ```
