These functions are used to query resource metadata to support efficient, automated data workflows.

# **GET Date Partitions**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_date_partitions`
_**ID**_ | `bqtools.[region].get_date_partitions`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Returns an array of dates corresponding to all existing partitions in a single partitioned table, sorted by descending date.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT partitions ARRAY<DATE>`
_**Returns**_ | `OUT partitions ARRAY<DATE>`
_**Dependencies**_ | `bqtools-qb.[region].get_date_partitions`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE partitioned_table_id STRING;
        DECLARE partitions ARRAY<DATE>;

        SET partitioned_table_id = 'project_id.dataset_name.partitioned_table_name';

        CALL bqtools.eu.get_date_partitions(partitioned_table_id, partitions);
        ```
    
    === "US"
        ```sql
        DECLARE partitioned_table_id STRING;
        DECLARE partitions ARRAY<DATE>;

        SET partitioned_table_id = 'project_id.dataset_name.partitioned_table_name';

        CALL bqtools.us.get_date_partitions(partitioned_table_id, partitions);
        ```

# **GET First Date Partition**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_date_partition`
_**ID**_ | `bqtools.[region].get_first_date_partition`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Returns the first partition date from a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT first_partition DATE`
_**Returns**_ | `OUT first_partition DATE`
_**Dependencies**_ | `bqtools.[region].get_date_partitions`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE partitioned_table_id STRING;
        DECLARE first_partition DATE;

        SET partitioned_table_id = 'project_id.dataset_name.partitioned_table_name';

        CALL bqtools.eu.get_first_date_partition(partitioned_table_id, first_partition);
        ```
    
    === "US"
        ```sql
        DECLARE partitioned_table_id STRING;
        DECLARE first_partition DATE;

        SET partitioned_table_id = 'project_id.dataset_name.partitioned_table_name';

        CALL bqtools.us.get_first_date_partition(partitioned_table_id, first_partition);
        ```

# **GET Last Date Partition**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_date_partition`
_**ID**_ | `bqtools.[region].get_last_date_partition`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Returns the last partition date from a single date-partitioned table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `partitioned_table_id STRING, OUT last_partition DATE`
_**Returns**_ | `OUT last_partition DATE`
_**Dependencies**_ | `bqtools.[region].get_date_partitions`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE partitioned_table_id STRING;
        DECLARE last_partition DATE;

        SET partitioned_table_id = 'project_id.dataset_name.partitioned_table_name';

        CALL bqtools.eu.get_last_date_partition(partitioned_table_id, last_partition);
        ```
    
    === "US"
        ```sql
        DECLARE partitioned_table_id STRING;
        DECLARE last_partition DATE;

        SET partitioned_table_id = 'project_id.dataset_name.partitioned_table_name';

        CALL bqtools.us.get_last_date_partition(partitioned_table_id, last_partition);
        ```

# **GET Date Shards**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_date_shards`
_**ID**_ | `bqtools.[region].get_date_shards`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Returns an array of `shard_dates` corresponding to all existing date shards in a single date-sharded table, sorted by descending date.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT shard_dates ARRAY<DATE>`
_**Returns**_ | `OUT shard_dates ARRAY<DATE>`
_**Dependencies**_ | `bqtools-qb.[region].get_date_shards`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE sharded_table_dataset_id, sharded_table_prefix STRING;
        DECLARE shard_dates ARRAY<DATE>;

        SET sharded_table_dataset_id = 'project_id.dataset_name';
        SET sharded_table_prefix = 'my_table_prefix_';

        CALL bqtools.eu.get_date_shards(sharded_table_dataset_id, sharded_table_prefix, shard_dates);
        ```
    
    === "US"
        ```sql
        DECLARE sharded_table_dataset_id, sharded_table_prefix STRING;
        DECLARE shard_dates ARRAY<DATE>;

        SET sharded_table_dataset_id = 'project_id.dataset_name';
        SET sharded_table_prefix = 'my_table_prefix_';

        CALL bqtools.us.get_date_shards(sharded_table_dataset_id, sharded_table_prefix, shard_dates);
        ```

# **GET First Date Shard**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_first_date_shard`
_**ID**_ | `bqtools.[region].get_first_date_shard`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Returns the first `shard_date` from a single sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT first_shard DATE`
_**Returns**_ | `OUT first_shard DATE`
_**Dependencies**_ | `bqtools.[region].get_date_shards`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE sharded_table_dataset_id, sharded_table_prefix STRING;
        DECLARE first_shard DATE;

        SET sharded_table_dataset_id = 'project_id.dataset_name';
        SET sharded_table_prefix = 'my_table_prefix_';

        CALL bqtools.eu.get_first_date_shard(sharded_table_dataset_id, sharded_table_prefix, first_shard);
        ```
    
    === "US"
        ```sql
        DECLARE sharded_table_dataset_id, sharded_table_prefix STRING;
        DECLARE first_shard DATE;

        SET sharded_table_dataset_id = 'project_id.dataset_name';
        SET sharded_table_prefix = 'my_table_prefix_';

        CALL bqtools.us.get_first_date_shard(sharded_table_dataset_id, sharded_table_prefix, first_shard);
        ```

# **GET Last Date Shard**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `get_last_date_shard`
_**ID**_ | `bqtools.[region].get_last_date_shard`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Returns the last `shard_date` from a single date-sharded table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `sharded_table_dataset_id STRING, sharded_table_prefix STRING, OUT last_shard DATE`
_**Returns**_ | `OUT last_shard DATE`
_**Dependencies**_ | `bqtools.[region].get_date_shards`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE sharded_table_dataset_id, sharded_table_prefix STRING;
        DECLARE last_shard DATE;

        SET sharded_table_dataset_id = 'project_id.dataset_name';
        SET sharded_table_prefix = 'my_table_prefix_';

        CALL bqtools.eu.get_last_date_shard(sharded_table_dataset_id, sharded_table_prefix, last_shard);
        ```
    
    === "US"
        ```sql
        DECLARE sharded_table_dataset_id, sharded_table_prefix STRING;
        DECLARE last_shard DATE;

        SET sharded_table_dataset_id = 'project_id.dataset_name';
        SET sharded_table_prefix = 'my_table_prefix_';

        CALL bqtools.us.get_last_date_shard(sharded_table_dataset_id, sharded_table_prefix, last_shard);
        ```