These functions are used to create resources from arbitrary SQL definitions and options.

# **`create_table`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table`
_**ID**_ | `bqtools.[region].create_table`
_**Description**_ | Creates or replaces a single `BASE TABLE` defined by an arbitrary sql `query_statement`, with options defined in alignment with the `CREATE TABLE` [DDL statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_table_statement). See the example below for `table_options` structure.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING, query_statement STRING, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table`

!!! warning "Beta"
    This `create_table` function is in beta as only the [`partition_expression`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#partition_expression) and [`clustering_column_list`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#clustering_column_list) options of the `table_options` have been implemented. See the example below for the JSON argument structure.

!!! info "execution: `create_table`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_table(table_id, query_statement, table_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_table(table_id, query_statement, table_options);
        ```

??? abstract "example: `create_table`"
    === "Partitioned and Clustered Table"
        ```sql
        DECLARE table_id, query_statement STRING;
        DECLARE table_options JSON;

        SET table_id = 'project_id.dataset_name.destination_table_name';
        SET query_statement = 'SELECT * FROM `project_id.dataset_name.source_table_name`';

        SET table_options = JSON """
        {
         "partition_expression": "DATE(_PARTITIONTIME)",
         "clustering_column_list": "column_a, column_b"
        }   
        """;

        CALL bqtools.[region].create_table(table_id, query_statement, table_options);
        ```
    
# **`create_view`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_view`
_**ID**_ | `bqtools.[region].create_view`
_**Description**_ | Creates or replaces a single `VIEW` defined by an arbitrary sql `query_statement`, with options defined in alignment with the `CREATE VIEW` [DDL statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_view_statement). 
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `view_id STRING, query_statement STRING, view_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_view`

!!! warning "Beta"
    This `create_view` function is in beta as not all of the `view_options` have been implemented.

!!! info "execution: `create_view`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_view(view_id, query_statement, view_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_view(view_id, query_statement, view_options);
        ```

# **`create_table_from_table_function`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table_from_table_function`
_**ID**_ | `bqtools.[region].create_table_from_table_function`
_**Description**_ | Creates or replaces a single `TABLE` defined by a date-parameterised `TABLE FUNCTION`, with options defined in alignment with the `CREATE TABLE FUNCTION` [DDL statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_table_function_statement).
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, source_table_function_id STRING, start_date DATE, end_date DATE, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table_from_table_function`, `bqtools-qb.[region].create_table`

!!! warning "Beta"
    This `create_table_from_table_function` function is in beta as only the [`partition_expression`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#partition_expression) and [`clustering_column_list`](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#clustering_column_list) options of the `table_options` have been implemented. See the example below for the JSON argument structure.

!!! info "execution: `create_table_from_table_function`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_table_from_table_function(destination_table_id, source_table_function_id, start_date, end_date, table_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_table_from_table_function(destination_table_id, source_table_function_id, start_date, end_date, table_options);
        ```

??? abstract "example: `create_table_from_table_function`"
    === "Partitioned and Clustered Table"
        ```sql
        DECLARE destination_table_id STRING;
        DECLARE source_table_function_id STRING;
        DECLARE start_date DATE;
        DECLARE end_date DATE;
        DECLARE table_options JSON;

        SET destination_table_id = 'project_id.dataset_name.destination_table_name';
        SET source_table_function_id = 'project_id.dataset_name.table_function_name';
        SET start_date = CURRENT_DATE - 7;
        SET end_date = CURRENT_DATE;

        SET table_options = JSON """
        {
         "partition_expression": "DATE(_PARTITIONTIME)",
         "clustering_column_list": "column_a, column_b"
        }   
        """;

        SELECT `bqtools-qb.[region].create_table_from_table_function`(destination_table_id, source_table_function_id, start_date, end_date, table_options);
        ```

# **`create_function`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_function`
_**ID**_ | `bqtools.[region].create_function`
_**Description**_ | Creates or replaces a `FUNCTION` defined by an arbitrary `query_statement`, with optional argument names and data types defined by the `arguments` `STRUCT ARRAY` and options defined in alignment with the `CREATE FUNCTION` [DDL statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_function_statement).
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `function_id STRING, query_statement STRING, arguments ARRAY<STRUCT<name STRING, data_type STRING>>, function_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_function`

!!! info "execution: `create_function`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_function(function_id, query_statement, arguments, function_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_function(function_id, query_statement, arguments, function_options);
        ```