These functions support profiling and working with [partitioned tables](https://cloud.google.com/bigquery/docs/partitioned-tables).

# **`get_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_date_partitions`
_**ID**_ | `bqtools.[region].get_date_partitions`
_**Description**_ | Returns an array of dates corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT partitions ARRAY<DATE>`
_**Returns**_ | `OUT partitions ARRAY<DATE>`
_**Dependencies**_ | `bqtools-qb.[region].get_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_date_partitions`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_date_partitions(partitioned_table_id, partitions);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_date_partitions(partitioned_table_id, partitions);
        ```

# **`get_date_partition_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_date_partition_ids`
_**ID**_ | `bqtools.[region].get_date_partition_ids`
_**Description**_ | Returns an array of `date_ids` corresponding to all existing partitions in a single partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT partition_ids ARRAY<STRING>`
_**Returns**_ | `OUT partition_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools-qb.[region].get_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_date_partition_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_date_partition_ids(partitioned_table_id, partition_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_date_partition_ids(partitioned_table_id, partition_ids);
        ```

# **`get_first_date_partition`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_date_partition`
_**ID**_ | `bqtools.[region].get_first_date_partition`
_**Description**_ | Returns the first partition date from a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT first_partition DATE`
_**Returns**_ | `OUT first_partition DATE`
_**Dependencies**_ | `bqtools.[region].get_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_first_date_partition`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_first_date_partition(partitioned_table_id, first_partition);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_first_date_partition(partitioned_table_id, first_partition);
        ```

# **`get_first_date_partition_id`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_date_partition_id`
_**ID**_ | `bqtools.[region].get_first_date_partition_id`
_**Description**_ | Returns the first partition `date_id` from a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT first_partition_id STRING`
_**Returns**_ | `OUT first_partition_id STRING`
_**Dependencies**_ | `bqtools.[region].get_date_partition_ids`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_first_date_partition_id`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_first_date_partition_id(partitioned_table_id, first_partition_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_first_date_partition_id(partitioned_table_id, first_partition_id);
        ```

# **`get_last_date_partition`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_date_partition`
_**ID**_ | `bqtools.[region].get_last_date_partition`
_**Description**_ | Returns the last partition date from a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT last_partition DATE`
_**Returns**_ | `OUT last_partition DATE`
_**Dependencies**_ | `bqtools.[region].get_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_last_date_partition`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_last_date_partition(partitioned_table_id, last_partition);
        ```
    
    === "US"
        ```sql
        CALL bqtools.us.get_last_date_partition(partitioned_table_id, last_partition);
        ```

# **`get_last_date_partition_id`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_date_partition_id`
_**ID**_ | `bqtools.[region].get_last_date_partition_id`
_**Description**_ | Returns the last partition `date_id` from a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT last_partition_id STRING`
_**Returns**_ | `OUT last_partition_id STRING`
_**Dependencies**_ | `bqtools.[region].get_date_partition_ids`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_last_date_partition_id`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_last_date_partition_id(partitioned_table_id, last_partition_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_last_date_partition_id(partitioned_table_id, last_partition_id);
        ```

# **`get_missing_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_missing_date_partitions`
_**ID**_ | `bqtools.[region].get_missing_date_partitions`
_**Description**_ | Returns an array containing the missing date partitions between the `start_date` and `end_date` in a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, start_date DATE, end_date DATE, OUT missing_partitions ARRAY<DATE>`
_**Returns**_ | `OUT missing_partitions ARRAY<DATE>`
_**Dependencies**_ | `bqtools.[region].get_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_missing_date_partitions`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_missing_date_partitions(partitioned_table_id, start_date, end_date, missing_partitions);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_missing_date_partitions(partitioned_table_id, start_date, end_date, missing_partitions);
        ```

# **`get_missing_date_partition_ids`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_missing_date_partition_ids`
_**ID**_ | `bqtools.[region].get_missing_date_partition_ids`
_**Description**_ | Returns an array containing the missing date `partition_ids` between the `start_date` and `end_date` in a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, start_date DATE, end_date DATE, OUT missing_partition_ids ARRAY<STRING>`
_**Returns**_ | `OUT missing_partition_ids ARRAY<STRING>`
_**Dependencies**_ | `bqtools.[region].get_date_partitions`, `bqtools.[region].parse_resource_id`

!!! info "execution: `get_missing_date_partition_ids`"
    === "EU"
        ```sql
        CALL bqtools.eu.get_missing_date_partition_ids(partitioned_table_id, start_date, end_date, missing_partition_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.get_missing_date_partition_ids(partitioned_table_id, start_date, end_date, missing_partition_ids);
        ```

# **`overwrite_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `overwrite_date_partitions`
_**ID**_ | `bqtools.[region].overwrite_date_partitions`
_**Description**_ | Writes a range of date partitions from a date-paramaterised table function into a destination table, deleting any existing destination table date partitions within the date range.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, date_column_name STRING, source_table_function_id STRING, start_date DATE, end_date DATE`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].get_last_date_partition`, `bqtools.[region].delete_date_partitions`, `bqtools.[region].insert_date_partitions`

!!! info "execution: `overwrite_date_partitions`"
    === "EU"
        ```sql
        CALL bqtools.eu.overwrite_date_partitions(destination_table_id, date_column_name, source_table_function_id, start_date, end_date);
        ```

    === "US"
        ```sql
        CALL bqtools.us.overwrite_date_partitions(destination_table_id, date_column_name, source_table_function_id, start_date, end_date);
        ```

# **`delete_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `delete_date_partitions`
_**ID**_ | `bqtools.[region].delete_date_partitions`
_**Description**_ | Deletes a range of date partitions from a date-partitioned destination table (with [time-unit column partitioning](https://cloud.google.com/bigquery/docs/partitioned-tables#date_timestamp_partitioned_tables)).
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, date_column_name STRING, start_date DATE, end_date DATE`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].delete_date_partitions`

!!! info "execution: `delete_date_partitions`"
    === "EU"
        ```sql
        CALL bqtools.eu.delete_date_partitions(destination_table_id, date_column_name, start_date, end_date);
        ```

    === "US"
        ```sql
        CALL bqtools.us.overwrite_date_partitions(destination_table_id, date_column_name, start_date, end_date);
        ```

# **`insert_date_partitions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `insert_date_partitions`
_**ID**_ | `bqtools.[region].insert_date_partitions`
_**Description**_ | Inserts a range of date-partitions from a date-paramaterised table function into a date-partitioned destination table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, source_table_function_id STRING, start_date DATE, end_date DATE`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].insert_date_partitions`

!!! info "execution: `delete_date_partitions`"
    === "EU"
        ```sql
        CALL bqtools.eu.delete_date_partitions(destination_table_id, source_table_function_id, start_date, end_date);
        ```

    === "US"
        ```sql
        CALL bqtools.us.overwrite_date_partitions(destination_table_id, source_table_function_id, start_date, end_date);
        ```