These functions are used to efficiently automate data flowing through BigQuery.

# **RUN flow**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `run_flow`
_**ID**_ | `bqtools.[region].run_flow`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Runs a single, idempotent execution of a logical flow, defined by a date-partitioned or date-sharded data source, a transformation/augmentation [date-bounded table function](/reference/bqtools/concepts/resources#date-bounded-table-function), a destination table and a set of execution and deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `source_table_id STRING, transform_resource_id STRING, destination_table_id STRING, execution_options JSON, destination_table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].parse_resource_id`, `bqtools.[region].get_first_date_shard`, `bqtools.[region].get_last_date_shard`, `bqtools.[region].get_first_date_partition`, `bqtools.[region].get_last_date_partition`, `bqtools.[region].create_table_from_date_bounded_table_function`, `bqtools.[region].replace_date_partitions_from_date_bounded_table_function`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE source_table_id, transform_resource_id, destination_table_id STRING;
        DECLARE execution_options,  destination_table_options JSON;

        SET source_table_id = 'project_id.analytics_1234.events_';
        SET transform_resource_id = 'project_id.analytics_1234.GA4_EVENTS';
        SET destination_table_id = 'project_id.analytics_1234.events';

        CALL bqtools.eu.run_flow(
            source_table_id, 
            transform_resource_id, 
            destination_table_id, 
            execution_options, 
            destination_table_options);
        ```
    
    === "US"
        ```sql
        DECLARE source_table_id, transform_resource_id, destination_table_id STRING;
        DECLARE execution_options,  destination_table_options JSON;

        SET source_table_id = 'project_id.analytics_1234.events_';
        SET transform_resource_id = 'project_id.analytics_1234.GA4_EVENTS';
        SET destination_table_id = 'project_id.analytics_1234.events';

        CALL bqtools.us.run_flow(
            source_table_id, 
            transform_resource_id, 
            destination_table_id, 
            execution_options, 
            destination_table_options);
        ```

!!! info "ARGUMENTS" 
    ARGUMENT | DATA TYPE | DESCRIPTION
    --- | --- | ---
    source_table_id | STRING | The source date-partitioned or date-sharded data table.
    transform_resource_id | STRING | The source date-bounded table function defining the transformation.
    destination_table_id | STRING | The destination date-partitioned table.
    execution_options | JSON | Options to configure specific executions.
    destination_table_options | JSON | Deployment options for the destination table.


??? note "EXECUTION OPTIONS"
    === "Options"
        Option | Data Type | Values | JSON Path | Default | Description
        --- | --- | --- | --- | --- | ---
        EXECUTION MODE | STRING | `full`, `incremental`, `date_range` | execution_mode | `incremental` | Flow execution mode.
        DATES TO REPLACE | INT64 | Integer value | replace_additional_date_partitions | `0` | Additional historic date partitions to replace in `incremental` mode.
        START DATE | DATE | Date value | start_date | Start date in `date_range` mode.
        END DATE | DATE | Date value | end_date | End date in `date_range` mode.
    
    Note that a `NULL` value for the `execution` mode will execute an `incremental` flow on only the latest arriving date partitions. 

??? note "DESTINATION TABLE OPTIONS"

    === "Destination Table Options"
        Option | Data Type | Values | JSON Path | Default | Description
        --- | --- | --- | --- | --- | ---
        PARTITION EXPRESSION | STRING | Partition expression or column | partition_expression | `None` | Destination table partitioning specification.
        CLUSTERING COLUMN LIST | ARRAY<STRING> |  Clustering columns | clustering_column_list | `None` | Destination table clustering specification.   
        DESCRIPTION | STRING |  Description | description | `None` | Destination table description.       
        