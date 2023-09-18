These functions are used to extend the accessibility of metadata returned from the [INFORMATION_SCHEMA](https://cloud.google.com/bigquery/docs/information-schema-intro) to permit aggregation of metadata across multiple projects.  These functions write the metadata to a destination table to avoid metadata query limits and improve subsequent query performance.

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