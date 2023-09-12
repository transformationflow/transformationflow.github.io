These functions support profiling and working with [date-sharded tables](https://cloud.google.com/bigquery/docs/partitioned-tables#dt_partition_shard).

# Functions
## **`get_table_date_shards`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_table_date_shards`
_**ID**_ | `bqtools.[region].get_table_date_shards`
_**Description**_ | Returns an array of dates corresponding to all existing date-sharded tables in a single sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT shard_dates ARRAY<DATE>`
_**Returns**_ | `OUT shard_dates ARRAY<DATE>`
_**Dependencies**_ | `bqtools-qb.[region].get_table_date_shards`

!!! info "execution: `get_table_date_shards`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_table_date_shards(sharded_table_dataset_id, sharded_table_prefix, shard_dates);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_table_date_shards(sharded_table_dataset_id, sharded_table_prefix, shard_dates);
        ```

## **`get_table_date_shard_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_table_date_shard_ids`
_**ID**_ | `bqtools.[region].get_table_date_shard_ids`
_**Description**_ | Returns an array of shard_ids corresponding to all existing date-sharded tables in a single sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT shard_ids ARRAY<STRING>`
_**Returns**_ | `OUT shard_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_table_date_shards`

!!! info "execution: `get_table_date_shard_ids`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_table_date_shard_ids(sharded_table_dataset_id, sharded_table_prefix, shard_ids);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_table_date_shard_ids(sharded_table_dataset_id, sharded_table_prefix, shard_ids);
        ```
