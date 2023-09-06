These functions are used to extend the accessibility of metadata returned from the [INFORMATION_SCHEMA](https://cloud.google.com/bigquery/docs/information-schema-intro) to permit aggregation of metadata across multiple projects.  These functions typically write the metadata to a destination table to avoid metadata query limits and improve subsequent query performance.

# Configuration
## **`PROJECT_IDS`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `PROJECT_IDS`
_**ID**_ | `BQMANAGER.PROJECT_IDS`
_**Description**_ | Configuration function used to determine which `project_ids` are included in the cross-project `INFORMATION_SCHEMA` functions.
_**Type**_ | `USER DEFINED FUNCTION`
_**Arguments**_ | `None`
_**Returns**_ | `project_ids ARRAY<STRING>`
_**Dependencies**_ | `None`

!!! info "configuration: `PROJECT_IDS`"
    
    ```sql
    CREATE OR REPLACE FUNCTION [destination_project_id].BQMANAGER.PROJECT_IDS()
    AS (
    [
    'project_id_1',
    'project_id_2',
    ...
    ]    
    )
    ```

!!! info "execution: `PROJECT_IDS`"
    ```sql
    SELECT [project_id].BQMANAGER.PROJECT_IDS();
    ```

# Common Arguments

## **`destination_project_id`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `destination_project_id`
_**Data Type**_ | `STRING`
_**Description**_ | The `project_id` containing the `BQMANAGER` dataset used to deploy BigQuery Manager functions.

!!! info "definition: `destination_project_id`"
    ```sql
    SET destination_project_id = 'project_id.dataset_name.table_name'
    ```

## **`source_project_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `source_project_ids`
_**Data Type**_ | `ARRAY<STRING>`
_**Description**_ | The `project_ids` across which the `INFORMATION_SCHEMA` function will aggregate the metadata.

!!! info "definition: `source_project_ids`"
    ```sql
    SET source_project_ids = [
    'project_id_1', 
    'project_id_2', 
    'project_id_3'
    ];
    ```
    
    This variable can also be set from the `PROJECT_IDS` configuration function:
    
    ```sql
    SET source_project_ids = (SELECT [project_id].BQMANAGER.PROJECT_IDS());
    ```

    It can also be passed directly into a function or procedure as a parenthesised argument:
    
    ```sql
    CALL BQMANAGER.EXAMPLE_PROCEDURE (
    destination_project_id,
    (SELECT [project_id].BQMANAGER.PROJECT_IDS()), 
    dataset_batch_size, 
    exclude_dataset_ids
    );
    ```


## **`dataset_batch_size`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `dataset_batch_size`
_**Data Type**_ | `INT64`
_**Description**_ | The number of datasets processed in each batch. It is used to tune the performance as some metadata queries will hit limits and fail if there is a large number of tables in the dataset batch. Setting this to a lower value will result in a more robust but longer-running procedure.

!!! info "definition: `dataset_batch_size`"
    ```sql
    SET dataset_batch_size = 100;
    ```

## **`exclude_dataset_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `exclude_dataset_ids`
_**Data Type**_ | `ARRAY<STRING>`
_**Description**_ | Specific datasets to exclude from the metadata query.  This can be helpful where a large number of tables in a dataset can cause an unrelated metadata query to crash (e.g. an extremely high of table shards in a dataset will cause an `INFORMATION_SCHEMA.PARTITIONS` query on the dataset to fail).

!!! info "definition: `exclude_dataset_ids`"
    ```sql
    SET exclude_dataset_ids = [
    'project_id.dataset_name_1',    
    'project_id.dataset_name_2'
    ];
    ```

# Functions
## **`TABLES`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `INFORMATION_SCHEMA_TABLES`
_**ID**_ | `bqtools.[region].INFORMATION_SCHEMA_TABLES`
_**Description**_ | Creates or replaces the `BQMANAGER.INFORMATION_SCHEMA_TABLES` table, which is the aggregation of the [`INFORMATION_SCHEMA.TABLES`](https://cloud.google.com/bigquery/docs/information-schema-tables) metadata view across multiple projects.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_project_id STRING, source_project_ids ARRAY<STRING>, dataset_batch_size INT64, exclude_dataset_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].INFORMATION_SCHEMA_TABLES`

!!! info "execution: `INFORMATION_SCHEMA_TABLES`"
    === "EU"
        ```sql
        CALL bqmanager.eu.INFORMATION_SCHEMA_TABLES(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

    === "US"
        ```sql
        CALL bqmanager.us.INFORMATION_SCHEMA_TABLES(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

## **`TABLES_METADATA`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `INFORMATION_SCHEMA_TABLES_METADATA`
_**ID**_ | `bqtools.[region].INFORMATION_SCHEMA_TABLES_METADATA`
_**Description**_ | Creates or replaces the `BQMANAGER.INFORMATION_SCHEMA_TABLES_METADATA` table, which is the aggregation of the `INFORMATION_SCHEMA.__TABLES__` metadata view across multiple projects.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_project_id STRING, source_project_ids ARRAY<STRING>, dataset_batch_size INT64, exclude_dataset_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].INFORMATION_SCHEMA_TABLES_METADATA`

!!! info "execution: `INFORMATION_SCHEMA_TABLES_METADATA`"
    === "EU"
        ```sql
        CALL bqmanager.eu.INFORMATION_SCHEMA_TABLES_METADATA(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

    === "US"
        ```sql
        CALL bqmanager.us.INFORMATION_SCHEMA_TABLES_METADATA(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

## **`TABLE_OPTIONS`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `INFORMATION_SCHEMA_TABLE_OPTIONS`
_**ID**_ | `bqtools.[region].INFORMATION_SCHEMA_TABLE_OPTIONS`
_**Description**_ | Creates or replaces the `BQMANAGER.INFORMATION_SCHEMA_TABLE_OPTIONS` table, which is the aggregation of the [`INFORMATION_SCHEMA.TABLE_OPTIONS`](https://cloud.google.com/bigquery/docs/information-schema-table-options) metadata view across multiple projects.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_project_id STRING, source_project_ids ARRAY<STRING>, dataset_batch_size INT64, exclude_dataset_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].INFORMATION_SCHEMA_TABLE_OPTIONS`

!!! info "execution: `INFORMATION_SCHEMA_TABLE_OPTIONS`"
    === "EU"
        ```sql
        CALL bqmanager.eu.INFORMATION_SCHEMA_TABLE_OPTIONS(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

    === "US"
        ```sql
        CALL bqmanager.us.INFORMATION_SCHEMA_TABLE_OPTIONS(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

## **`PARTITIONS`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `INFORMATION_SCHEMA_PARTITIONS`
_**ID**_ | `bqtools.[region].INFORMATION_SCHEMA_PARTITIONS`
_**Description**_ | Creates or replaces the `BQMANAGER.INFORMATION_SCHEMA_PARTITIONS` table, which is the aggregation of the [`INFORMATION_SCHEMA.PARTITIONS`](https://cloud.google.com/bigquery/docs/information-schema-partitions) metadata view across multiple projects.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_project_id STRING, source_project_ids ARRAY<STRING>, dataset_batch_size INT64, exclude_dataset_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].INFORMATION_SCHEMA_PARTITIONS`

!!! info "execution: `INFORMATION_SCHEMA_PARTITIONS`"
    === "EU"
        ```sql
        CALL bqmanager.eu.INFORMATION_SCHEMA_PARTITIONS(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```

    === "US"
        ```sql
        CALL bqmanager.us.INFORMATION_SCHEMA_PARTITIONS(destination_project_id, source_project_ids, dataset_batch_size, exclude_dataset_ids)
        ```