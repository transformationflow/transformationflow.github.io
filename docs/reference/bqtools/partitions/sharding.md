These functions support profiling and working with [date-sharded tables](https://cloud.google.com/bigquery/docs/partitioned-tables#dt_partition_shard).

# Functions
## **`get_date_shards`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_date_shards`
_**ID**_ | `bqtools.[region].get_date_shards`
_**Description**_ | Returns an array of `shard_dates` corresponding to all existing date shards in a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT shard_dates ARRAY<DATE>`
_**Returns**_ | `OUT shard_dates ARRAY<DATE>`
_**Dependencies**_ | `bqtools-qb.[region].get_date_shards`

!!! info "execution: `get_date_shards`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_date_shards(sharded_table_dataset_id, sharded_table_prefix, shard_dates);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_date_shards(sharded_table_dataset_id, sharded_table_prefix, shard_dates);
        ```

## **`get_date_shard_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_date_shard_ids`
_**ID**_ | `bqtools.[region].get_date_shard_ids`
_**Description**_ | Returns an array of `shard_ids` corresponding to all existing date shards in a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT shard_ids ARRAY<STRING>`
_**Returns**_ | `OUT shard_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_date_shards`

!!! info "execution: `get_date_shard_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_date_shard_ids(sharded_table_dataset_id, sharded_table_prefix, shard_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_date_shard_ids(sharded_table_dataset_id, sharded_table_prefix, shard_ids);
        ```

## **`get_first_date_shard`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_date_shard`
_**ID**_ | `bqtools.[region].get_first_date_shard`
_**Description**_ | Returns the first `shard_date` from a single sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT first_shard DATE`
_**Returns**_ | `OUT first_shard DATE`
_**Dependencies**_ | `bqtools.[region].get_date_shards`, `bqtools-qb.[region].get_date_shards`

!!! info "execution: `get_first_date_shard`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_first_date_shard(sharded_table_dataset_id, sharded_table_prefix, first_shard);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_first_date_shard(sharded_table_dataset_id, sharded_table_prefix, first_shard);
        ```

## **`get_first_date_shard_id`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_date_shard_id`
_**ID**_ | `bqtools.[region].get_first_date_shard_id`
_**Description**_ | Returns the first `shard_id` from a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT first_shard_id STRING`
_**Returns**_ | `OUT first_shard_id STRING`
_**Dependencies**_ | `bqtools.[region].get_date_shard_ids`, `bqtools-qb.[region].get_date_shards`

!!! info "execution: `get_first_date_shard_id`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_first_date_shard_id(sharded_table_dataset_id, sharded_table_prefix, first_shard_id);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_first_date_shard_id(sharded_table_dataset_id, sharded_table_prefix, first_shard_id);
        ```

## **`get_last_date_shard`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_date_shard`
_**ID**_ | `bqtools.[region].get_last_date_shard`
_**Description**_ | Returns the last `shard_date` from a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT last_shard DATE`
_**Returns**_ | `OUT last_shard DATE`
_**Dependencies**_ | `bqtools.[region].get_date_shards`, `bqtools-qb.[region].get_date_shards`

!!! info "execution: `get_last_date_shard`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_last_date_shard(sharded_table_dataset_id, sharded_table_prefix, last_shard);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_last_date_shard(sharded_table_dataset_id, sharded_table_prefix, last_shard);
        ```

## **`get_last_date_shard_id`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_date_shard_id`
_**ID**_ | `bqtools.[region].get_last_date_shard_id`
_**Description**_ | Returns the last `shard_id` from a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT last_shard_id STRING`
_**Returns**_ | `OUT last_shard_id STRING`
_**Dependencies**_ | `bqtools.[region].get_date_shard_ids`, `bqtools-qb.[region].get_date_shard_ids`

!!! info "execution: `get_last_date_shard_id`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_last_date_shard_id(sharded_table_dataset_id, sharded_table_prefix, last_shard_id);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_last_date_shard_id(sharded_table_dataset_id, sharded_table_prefix, last_shard_id);
        ```

## **`get_missing_date_shards`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_missing_date_shards`
_**ID**_ | `bqtools.[region].get_missing_date_shards`
_**Description**_ | Returns an array containing the missing `date_shards` between the `start_date` and `end_date` in a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, start_date DATE, end_date DATE, OUT missing_shards ARRAY<DATE>`
_**Returns**_ | `OUT missing_shards ARRAY<DATE>`
_**Dependencies**_ | `bqtools.[region].get_date_shards`, `bqtools-qb.[region].get_date_shards`

!!! info "execution: `get_missing_date_shards`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_missing_date_shards(sharded_table_dataset_id, sharded_table_prefix, start_date, end_date, missing_shards);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_missing_date_shards(sharded_table_dataset_id, sharded_table_prefix, start_date, end_date, missing_shards);
        ```
    
## **`get_missing_date_shard_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_missing_date_shard_ids`
_**ID**_ | `bqtools.[region].get_missing_date_shard_ids`
_**Description**_ | Returns an array containing the missing `shard_ids` between the `start_date` and `end_date` in a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, start_date DATE, end_date DATE, OUT missing_shard_ids ARRAY<STRING>`
_**Returns**_ | `OUT missing_shard_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools.[region].get_date_shard_ids`, `bqtools-qb.[region].get_date_shard_ids`

!!! info "execution: `get_missing_date_shard_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_missing_date_shard_ids(sharded_table_dataset_id, sharded_table_prefix, start_date, end_date, missing_shard_ids);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_missing_date_shard_ids(sharded_table_dataset_id, sharded_table_prefix, start_date, end_date, missing_shard_ids);
        ```