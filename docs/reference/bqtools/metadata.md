These functions return metadata related to BigQuery resources to support data management and automation activities in the native BigQuery environment.

# Functions
## **`get_dataset_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_dataset_ids`
_**Description**_ | Returns the dataset_ids for all datasets across multiple projects
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `project_ids ARRAY<STRING>`
_**Returns**_ | `dataset_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_dataset_ids`

!!! info "execution: `get_dataset_ids`"
    === "EU"
        ```sql
        DECLARE project_ids, dataset_ids ARRAY<STRING>;
        SET project_ids = ['project_a', 'project_b'];

        CALL bqtools.eu.get_dataset_ids(project_ids, dataset_ids);
        ```

    === "US"
        ```sql
        DECLARE project_ids, dataset_ids ARRAY<STRING>;
        SET project_ids = ['project_a', 'project_b'];
        
        CALL bqtools.us.get_dataset_ids(project_ids, dataset_ids);
        ```

## **`get_sql`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_sql`
_**Description**_ | Returns the SQL definition of a single view or routine
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `routine_or_view_id STRING`
_**Returns**_ | `sql STRING`
_**Dependencies**_ | `bqtools-qb.[region].get_sql`

!!! info "execution: `get_sql`"
    === "EU"
        ```sql
        DECLARE routine_or_view_id, sql STRING;
        SET routine_or_view_id = 'project_id.dataset_name.routine_or_view_name';

        CALL bqtools.eu.get_sql(routine_or_view_id, sql);
        ```

    === "US"
        ```sql
        DECLARE routine_or_view_id, sql STRING;
        SET routine_or_view_id = 'project_id.dataset_name.routine_or_view_name';
        
        CALL bqtools.us.get_sql(routine_or_view_id, sql);
        ```