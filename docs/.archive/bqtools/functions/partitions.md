These functions support creation, insertion and deletion of data partitions to support composition of idempotent data operations.

# **DELETE Date Partitions**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `delete_date_partitions`
_**ID**_ | `bqtools.[region].delete_date_partitions`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Deletes a range of date-partitions from a date-partitioned destination table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING, date_column_name STRING, start_date DATE, end_date DATE`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].delete_date_partitions`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE destination_table_id, date_column_name STRING;
        DECLARE start_date, end_date DATE;

        SET destination_table_id = 'project_id.dataset_name.partitioned_table_name';
        SET date_column_name = 'event_date';
        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        CALL bqtools.eu.delete_date_partitions(destination_table_id, event_date, date_column_name, start_date, end_date);
        ```
    
    === "US"
        ```sql
        DECLARE destination_table_id, date_column_name STRING;
        DECLARE start_date, end_date DATE;

        SET destination_table_id = 'project_id.dataset_name.partitioned_table_name';
        SET date_column_name = 'event_date';
        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        CALL bqtools.us.delete_date_partitions(destination_table_id, event_date, date_column_name, start_date, end_date);
        ```

# **INSERT Date Partitions**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `insert_date_partitions_from_date_bounded_table_function`
_**ID**_ | `bqtools.[region].insert_date_partitions_from_date_bounded_table_function`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Inserts a range of date partitions from a date-bounded table function into a date-partitioned destination table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, source_table_function_id STRING, start_date DATE, end_date DATE`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].insert_date_partitions_from_date_bounded_table_function`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE destination_table_id, source_table_function_id STRING;
        DECLARE start_date, end_date DATE;

        SET destination_table_id = 'project_id.dataset_name.partitioned_table_name';
        SET source_table_function_id = 'project_id.dataset_name.table_function_name';

        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        CALL bqtools.eu.insert_date_partitions_from_date_bounded_table_function(destination_table_id, source_table_function_id, start_date, end_date);
        ```
    
    === "US"
        ```sql
        DECLARE destination_table_id, source_table_function_id STRING;
        DECLARE start_date, end_date DATE;

        SET destination_table_id = 'project_id.dataset_name.partitioned_table_name';
        SET source_table_function_id = 'project_id.dataset_name.table_function_name';

        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        CALL bqtools.us.insert_date_partitions_from_date_bounded_table_function(destination_table_id, source_table_function_id, start_date, end_date);
        ```

# **REPLACE Date Partitions**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `replace_date_partitions_from_date_bounded_table_function`
_**ID**_ | `bqtools.[region].replace_date_partitions_from_date_bounded_table_function`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Deletes a range of date partitions from a date-partitioned destination table and inserts the same range of date partitions from a date-bounded table function.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, date_column_name STRING, source_table_function_id STRING, start_date DATE, end_date DATE`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].delete_date_partitions`, `bqtools.[region].insert_date_partitions_from_date_bounded_table_function`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE destination_table_id, date_column_name, source_table_function_id STRING;
        DECLARE start_date, end_date DATE;

        SET destination_table_id = 'project_id.dataset_name.partitioned_table_name';
        SET date_column_name = 'event_date';
        SET source_table_function_id = 'project_id.dataset_name.table_function_name';

        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        CALL bqtools.eu.replace_date_partitions_from_date_bounded_table_function(destination_table_id, date_column_name, source_table_function_id, start_date, end_date);
        ```
    
    === "US"
        ```sql
        DECLARE destination_table_id, date_column_name, source_table_function_id STRING;
        DECLARE start_date, end_date DATE;

        SET destination_table_id = 'project_id.dataset_name.partitioned_table_name';
        SET date_column_name = 'event_date';
        SET source_table_function_id = 'project_id.dataset_name.table_function_name';

        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        CALL bqtools.us.replace_date_partitions_from_date_bounded_table_function(destination_table_id, date_column_name, source_table_function_id, start_date, end_date);
        ```