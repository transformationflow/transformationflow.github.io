These functions support profiling and working with [partitioned tables](https://cloud.google.com/bigquery/docs/partitioned-tables).

# Functions
## **`get_table_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_table_date_partitions`
_**ID**_ | `bqtools.[region].get_table_date_partitions`
_**Description**_ | Returns an array of dates corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT partitions ARRAY<DATE>`
_**Returns**_ | `OUT partitions ARRAY<DATE>`
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

## **`get_first_table_date_partition`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_table_date_partition`
_**ID**_ | `bqtools.[region].get_first_table_date_partition`
_**Description**_ | Returns the first date partition corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT first_partition DATE`
_**Returns**_ | `OUT first_partition DATE`
_**Dependencies**_ | `bqtools.[region].get_table_date_partitions`, `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_first_table_date_partition`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_first_table_date_partition(partitioned_table_id, first_partition);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_first_table_date_partition(partitioned_table_id, first_partition);
        ```

## **`get_first_table_date_partition_id`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_table_date_partition_id`
_**ID**_ | `bqtools.[region].get_first_table_date_partition_id`
_**Description**_ | Returns the first date partition id corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT first_partition_id STRING`
_**Returns**_ | `OUT first_partition_id STRING`
_**Dependencies**_ | `bqtools.[region].get_table_date_partition_ids`, `bqtools-bq.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_first_table_date_partition_id`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_first_table_date_partition_id(partitioned_table_id, first_partition_id);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_first_table_date_partition_id(partitioned_table_id, first_partition_id);
        ```

## **`get_last_table_date_partition`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_table_date_partition`
_**ID**_ | `bqtools.[region].get_last_table_date_partition`
_**Description**_ | Returns the last date partition corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT last_partition DATE`
_**Returns**_ | `OUT last_partition DATE`
_**Dependencies**_ | `bqtools.[region].get_table_date_partitions`, `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_last_table_date_partition`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_last_table_date_partition(partitioned_table_id, last_partition);
        ```
    
    === "US"
        ```sql
        SELECT bqtools.us.get_last_table_date_partition(partitioned_table_id, last_partition);
        ```

## **`get_last_table_date_partition_id`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_table_date_partition_id`
_**ID**_ | `bqtools.[region].get_last_table_date_partition_id`
_**Description**_ | Returns the last date partition id corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT last_partition_id STRING`
_**Returns**_ | `OUT last_partition_id STRING`
_**Dependencies**_ | `bqtools.[region].get_table_date_partition_ids`, `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_last_table_date_partition_id`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_last_table_date_partition_id(partitioned_table_id, last_partition_id);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_last_table_date_partition_id(partitioned_table_id, last_partition_id);
        ```

## **`get_missing_table_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_missing_table_date_partitions`
_**ID**_ | `bqtools.[region].get_missing_table_date_partitions`
_**Description**_ | Returns an array of missing dates corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, start_date DATE, end_date DATE, OUT missing_partitions ARRAY<DATE>`
_**Returns**_ | `OUT missing_partitions ARRAY<DATE>`
_**Dependencies**_ | `bqtools.[region].get_table_date_partitions`, `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_missing_table_date_partitions`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_missing_table_date_partitions(partitioned_table_id, start_date, end_date, missing_partitions);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_missing_table_date_partitions(partitioned_table_id, start_date, end_date, missing_partitions);
        ```

## **`get_missing_table_date_partition_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_missing_table_date_partition_ids`
_**ID**_ | `bqtools.[region].get_missing_table_date_partition_ids`
_**Description**_ | Returns an array of missing date_ids corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, start_date DATE, end_date DATE, OUT missing_partition_ids ARRAY<STRING>`
_**Returns**_ | `OUT missing_partition_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools.[region].get_table_date_partitions`, `bqtools-qb.[region].get_table_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_missing_table_date_partition_ids`"
    === "EU"
        ```sql
        SELECT bqtools.eu.get_missing_table_date_partition_ids(partitioned_table_id, start_date, end_date, missing_partition_ids);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.get_missing_table_date_partition_ids(partitioned_table_id, start_date, end_date, missing_partition_ids);
        ```