These functions support profiling and working with [partitioned tables](https://cloud.google.com/bigquery/docs/partitioned-tables)...

# Functions
## **`get_table_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_table_date_partitions`
_**ID**_ | `bqtools.[region].get_table_date_partitions`
_**Description**_ | Returns an array of dates corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT partitions ARRAY<DATE>`
_**Returns**_ | `OUT partitions ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_table_date_partitions`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_table_date_partitions(partitioned_table_id, partitions);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_table_date_partitions(partitioned_table_id, partitions);
        ```

## **`get_table_date_partition_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_table_date_partition_ids`
_**ID**_ | `bqtools.[region].get_table_date_partition_ids`
_**Description**_ | Returns an array of date_ids corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT partition_ids ARRAY<STRING>`
_**Returns**_ | `OUT partition_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_table_date_partition_ids`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_table_date_partition_ids(partitioned_table_id, partition_ids);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_table_date_partition_ids(partitioned_table_id, partition_ids);
        ```

