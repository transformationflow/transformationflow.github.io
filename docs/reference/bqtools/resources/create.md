These functions are used to create resources from SQL definitions and options.

# **`create_function`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_function`
_**ID**_ | `bqtools.[region].create_function`
_**Description**_ | Creates or replaces a `SQL` User-Defined `FUNCTION` described by an arbitrary `SQL` `query_statement`, with each optional argument `name` and `data_type` defined by an array element in the `arguments` `STRUCT ARRAY`, and an optional function description defined in the `options` `JSON` argument.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `function_id STRING, query_statement STRING, function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>, function_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_function`

!!! info "execution: `create_function`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_function(function_id, query_statement, function_arguments, function_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_function(function_id, query_statement, function_arguments, function_options);
        ```

??? abstract "example: `create_function`"
    === "EU"
        ```sql
        --DECLARATION
          DECLARE function_id, query_statement STRING;
          DECLARE function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
          DECLARE function_options JSON;
    
        --CONFIGURATION
          SET function_id = 'project_id.dataset_name.function_name';
          SET function_arguments = [('argument_1', 'STRING'), ('argument_2', 'INT64')];
          SET function_options = JSON '{"description": "test description"}';
          SET query_statement = "SELECT 1";

        --EXECUTION
          CALL bqtools.eu.create_function(function_id, query_statement, function_arguments, function_options);
        ```

    === "US"
        ```sql
        --DECLARATION
          DECLARE function_id, query_statement STRING;
          DECLARE function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
          DECLARE function_options JSON;
    
        --CONFIGURATION
          SET function_id = 'project_id.dataset_name.function_name';
          SET function_arguments = [('argument_1', 'STRING'), ('argument_2', 'INT64')];
          SET function_options = JSON '{"description": "test description"}';
          SET query_statement = "SELECT 1";

        --EXECUTION
          CALL bqtools.us.create_function(function_id, query_statement, function_arguments, function_options);
        ```

# **`create_table_function`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table_function`
_**ID**_ | `bqtools.[region].create_table_function`
_**Description**_ | Creates or replaces a `TABLE FUNCTION` described by an arbitrary `SQL` `query_statement`, with each optional argument `name` and `data_type` defined by an array element in the `arguments` `STRUCT ARRAY`, and an optional table function description defined in the `table_function_options` `JSON` argument.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_function_id STRING, query_statement STRING, table_function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>, table_function_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_function`

!!! info "execution: `create_table_function`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```

??? abstract "example: `create_table_function`"
    === "EU"
        ```sql
        --DECLARATION
          DECLARE table_function_id, query_statement STRING;
          DECLARE table_function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
          DECLARE table_function_options JSON;
    
        --CONFIGURATION
          SET table_function_id = 'project_id.dataset_name.table_function_name';
          SET table_function_arguments = [('argument_1', 'STRING'), ('argument_2', 'INT64')];
          SET table_function_options = JSON '{"description": "test description"}';
          SET query_statement = "SELECT 1";

        --EXECUTION
          CALL bqtools.eu.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```

    === "US"
        ```sql
        --DECLARATION
          DECLARE table_function_id, query_statement STRING;
          DECLARE table_function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
          DECLARE table_function_options JSON;
    
        --CONFIGURATION
          SET table_function_id = 'project_id.dataset_name.table_function_name';
          SET table_function_arguments = [('argument_1', 'STRING'), ('argument_2', 'INT64')];
          SET table_function_options = JSON '{"description": "test description"}';
          SET query_statement = "SELECT 1";

        --EXECUTION
          CALL bqtools.us.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```