These functions are used to create resources from SQL definitions and deployment-specific options.

# **CREATE Function**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_function`
_**ID**_ | `bqtools.[region].create_function`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a sql user-defined function defined by an arbitrary query statement and a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `function_id STRING, query_statement STRING, function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>, function_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_function`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE function_id, query_statement STRING;
        DECLARE function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
        DECLARE function_options JSON;

        SET function_id = 'project_id.dataset_name.function_name';
        SET function_arguments = [("string_argument", "STRING")];
        SET query_statement = 'SELECT 1 AS example';

        SELECT bqtools.eu.create_function(function_id, query_statement, function_arguments, function_options);
        ```
    
    === "US"
        ```sql
        DECLARE function_id, query_statement STRING;
        DECLARE function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
        DECLARE function_options JSON DEFAULT JSON '{}';

        SET function_id = 'project_id.dataset_name.function_name';
        SET function_arguments = [("string_argument", "STRING")];
        SET query_statement = 'SELECT 1 AS example';

        SELECT bqtools.us.create_function(function_id, query_statement, function_arguments, function_options);
        ```

??? note "OPTIONS"
    === "Options"
        The following deployment options can be set using the `function_options` `JSON` argument, which is a subset of the [CREATE FUNCTION DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_function_statement) options.
        
        Option | Option Data Type | Values | JSON Path | Default
        --- | --- | --- | --- | ---
        REPLACE | `BOOLEAN` | `true`/`false`| `replace_function` | `true` 
        TEMPORARY | `BOOLEAN` | `true`/`false`| `temporary_function` | `false`
        IF NOT EXISTS | `BOOLEAN` | `true`/`false`| `if_not_exists` | `false`
        DESCRIPTION | `STRING` | `Function description` | `description` | `None`
    
    === "Default"
        If the `function_options` `JSON` argument is `NULL`, the default options will correspond to the following DDL statement:

        ```sql
        CREATE OR REPLACE FUNCTION `[function_id]`([function_arguments])
        AS
        [query_statement]
        ```

# **CREATE Procedure**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_procedure`
_**ID**_ | `bqtools.[region].create_procedure`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a sql user-defined function defined by an arbitrary query statement and a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `procedure_id STRING, query_statement STRING, procedure_arguments ARRAY<STRUCT<name STRING, data_type STRING>>, procedure_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_procedure`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE procedure_id, query_statement STRING;
        DECLARE procedure_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
        DECLARE procedure_options JSON DEFAULT JSON '{}';

        SET procedure_id = 'project_id.dataset_name.procedure_name';
        SET procedure_arguments = [("string_argument", "STRING")];

        SELECT bqtools.eu.create_procedure(procedure_id, query_statement, procedure_arguments, procedure_options);
        ```
    
    === "US"
        ```sql
        DECLARE procedure_id, query_statement STRING;
        DECLARE procedure_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
        DECLARE procedure_options JSON;

        SET procedure_id = 'project_id.dataset_name.procedure_name';
        SET procedure_arguments = [("string_argument", "STRING")];

        SELECT bqtools.us.create_procedure(procedure_id, query_statement, procedure_arguments, procedure_options);
        ```

??? note "OPTIONS"
    === "Options"
        The following deployment options can be set using the `procedure_options` `JSON` argument, which is a subset of the [CREATE PROCEDURE DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_procedure) options.
        
        Option | Option Data Type | Values | JSON Path | Default
        --- | --- | --- | --- | ---
        REPLACE | `BOOLEAN` | `true`/`false`| `replace_procedure` | `true` 
        TEMPORARY | `BOOLEAN` | `true`/`false`| `temporary_procedure` | `false`
        IF NOT EXISTS | `BOOLEAN` | `true`/`false`| `if_not_exists` | `false`
        DESCRIPTION | `STRING` | `Function description` | `description` | `None`
    
    === "Default"
        If the `procedure_options` `JSON` argument is `NULL`, the default options will correspond to the following DDL statement:

        ```sql
        CREATE OR REPLACE PROCEDURE `[procedure_id]`([procedure_arguments])
        AS
        [query_statement]
        ```

# **CREATE Table**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table`
_**ID**_ | `bqtools.[region].create_table`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a single base table as the results of an arbitrary sql statement and a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING, query_statement STRING, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE table_id, query_statement STRING;
        DECLARE table_options JSON;

        SET table_id = 'project_id.dataset_name.table_name';

        SELECT bqtools.eu.create_table(table_id, query_statement, table_options);
        ```
    
    === "US"
        ```sql
        DECLARE table_id, query_statement STRING;
        DECLARE table_options JSON DEFAULT JSON '{}';

        SET table_id = 'project_id.dataset_name.table_name';

        SELECT bqtools.us.create_table(table_id, query_statement, table_options);
        ```

??? note "OPTIONS"
    === "Options"
        The following deployment options can be set using the `table_options` `JSON` argument, which is a subset of the [CREATE TABLE DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_table_statement) options.
        
        Option | Option Data Type | Values | JSON Path | Default
        --- | --- | --- | --- | ---
        REPLACE | `BOOLEAN` | `true`/`false`| `replace_procedure` | `true` 
        TEMPORARY | `BOOLEAN` | `true`/`false`| `temporary_procedure` | `false`
        IF NOT EXISTS | `BOOLEAN` | `true`/`false`| `if_not_exists` | `false`
        DESCRIPTION | `STRING` | `Function description` | `description` | `None`
        PARTITION EXPRESSION | `STRING` | `Partition expression`|  `partition_expression` | `None`
        CLUSTERING COLUMN LIST | `ARRAY<STRING>` | `Clustering columns`|  `clustering_column_list` | `None`
        EXPIRATION TIMESTAMP | `STRING` | `Timestamp` | `expiration_timestamp` | `None`
        PARTITION EXPIRATION DAYS | `INTEGER` | `Integer` | `partition_expiration_days` | `None`
        REQUIRE PARTITION FILTER | `BOOLEAN` | `true`/`false` | `require_partition_filter` | `false`
        KMS KEY NAME | `STRING` | `KMS key name` | `kms_key_name` | `None`
        FRIENDLY NAME | `STRING` | `Friendly name` | `friendly_name` | `None`
        DEFAULT ROUNDING MODE | `STRING` | `Default rounding mode` | `default_rounding_mode` | `None`

    === "Default"
        If the `table_options` `JSON` argument is `NULL`, the default options will correspond to the following DDL statement:

        ```sql
        CREATE OR REPLACE TABLE `[procedure_id]`
        AS
        [query_statement]
        ```


# **CREATE Table Function**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table_function`
_**ID**_ | `bqtools.[region].create_table_function`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a table function defined by an arbitrary sql statement and a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_function_id STRING, query_statement STRING, table_function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>, table_function_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table_function`


!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE table_function_id, query_statement STRING;
        DECLARE table_function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
        DECLARE table_function_options JSON;

        SET table_function_id = 'project_id.dataset_name.table_function_name';
        SET table_function_arguments = [("string_argument", "STRING")];
        SET query_statement = 'SELECT 1 AS example';

        SELECT bqtools.eu.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```
    
    === "US"
        ```sql
        DECLARE table_function_id, query_statement STRING;
        DECLARE table_function_arguments ARRAY<STRUCT<name STRING, data_type STRING>>;
        DECLARE table_function_options JSON;

        SET table_function_id = 'project_id.dataset_name.table_function_name';
        SET table_function_arguments = [("string_argument", "STRING")];
        SET query_statement = 'SELECT 1 AS example';

        SELECT bqtools.us.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```

??? note "OPTIONS"
    === "Options"
        The following deployment options can be set using the `table_function_options` `JSON` argument, which is a subset of the [CREATE TABLE FUNCTION DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_table_function_statement) options.
        
        Option | Option Data Type | Values | JSON Path | Default
        --- | --- | --- | --- | ---
        REPLACE | `BOOLEAN` | `true`/`false`| `replace_procedure` | `true` 
        IF NOT EXISTS | `BOOLEAN` | `true`/`false`| `if_not_exists` | `false`
        DESCRIPTION | `STRING` | `Function description` | `description` | `None`
    
    === "Default"
        If the `function_options` `JSON` argument is `NULL`, the default options will correspond to the following DDL statement:

        ```sql
        CREATE OR REPLACE TABLE FUNCTION `[procedure_id]`([procedure_arguments])
        AS
        [query_statement]
        ```

# **CREATE Table from DBTF**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table_from_date_bounded_table_function`
_**ID**_ | `bqtools.[region].create_table_from_date_bounded_table_function`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a single base table as a date partition range from a date-bounded table function, with a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, source_table_function_id STRING, start_date DATE, end_date DATE, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE destination_table_id, source_table_function_id STRING;
        DECLARE start_date, end_date DATE;
        DECLARE table_options JSON;

        SET destination_table_id = 'project_id.dataset_name.destination_table_name';
        SET source_table_function_id = 'project_id.dataset_name.source_table_function_name';

        SELECT bqtools.eu.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```
    
    === "US"
        ```sql
        DECLARE destination_table_id, source_table_function_id STRING;
        DECLARE start_date, end_date DATE;
        DECLARE table_options JSON;

        SET destination_table_id = 'project_id.dataset_name.destination_table_name';
        SET source_table_function_id = 'project_id.dataset_name.source_table_function_name';

        SELECT bqtools.us.create_table_function(table_function_id, query_statement, table_function_arguments, table_function_options);
        ```

??? note "OPTIONS"
    === "Options"
        The following deployment options can be set using the `table_options` `JSON` argument, which is a subset of the [CREATE TABLE DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_table_statement) options.
        
        Option | Option Data Type | Values | JSON Path | Default
        --- | --- | --- | --- | ---
        REPLACE | `BOOLEAN` | `true`/`false`| `replace_procedure` | `true` 
        TEMPORARY | `BOOLEAN` | `true`/`false`| `temporary_procedure` | `false`
        IF NOT EXISTS | `BOOLEAN` | `true`/`false`| `if_not_exists` | `false`
        DESCRIPTION | `STRING` | `Function description` | `description` | `None`
        PARTITION EXPRESSION | `STRING` | `Partition expression`|  `partition_expression` | `None`
        CLUSTERING COLUMN LIST | `ARRAY<STRING>` | `Clustering columns`|  `clustering_column_list` | `None`
        EXPIRATION TIMESTAMP | `STRING` | `Timestamp` | `expiration_timestamp` | `None`
        PARTITION EXPIRATION DAYS | `INTEGER` | `Integer` | `partition_expiration_days` | `None`
        REQUIRE PARTITION FILTER | `BOOLEAN` | `true`/`false` | `require_partition_filter` | `false`
        KMS KEY NAME | `STRING` | `KMS key name` | `kms_key_name` | `None`
        FRIENDLY NAME | `STRING` | `Friendly name` | `friendly_name` | `None`
        DEFAULT ROUNDING MODE | `STRING` | `Default rounding mode` | `default_rounding_mode` | `None`

    === "Default"
        If the `table_options` `JSON` argument is `NULL`, the default options will correspond to the following DDL statement:

        ```sql
        CREATE OR REPLACE TABLE `[destination_table_id]`
        AS
        SELECT * FROM `[source_table_function_id]`([start_date], [end_date])
        ```

# **CREATE View**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_view`
_**ID**_ | `bqtools.[region].create_view`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a single view defined by an arbitrary sql statement and a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `view_id STRING, query_statement STRING, view_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_view`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE view_id, query_statement STRING;
        DECLARE view_options JSON;

        SET view_id = 'project_id.dataset_name.view_name';
        SET query_statement = 'SELECT 1 AS example';

        SELECT bqtools.eu.create_view(view_id, query_statement, view_options);
        ```
    
    === "US"
        ```sql
        DECLARE view_id, query_statement STRING;
        DECLARE view_options JSON;

        SET view_id = 'project_id.dataset_name.view_name';
        SET query_statement = 'SELECT 1 AS example';

        SELECT bqtools.us.create_view(view_id, query_statement, view_options);
        ```

??? note "OPTIONS"
    === "Options"
        The following deployment options can be set using the `view_options` `JSON` argument, which is a subset of the [CREATE VIEW DDL](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_view_statement) options.
        
        Option | Option Data Type | Values | JSON Path | Default
        --- | --- | --- | --- | ---
        REPLACE | `BOOLEAN` | `true`/`false`| `replace_view` | `true` 
        IF NOT EXISTS | `BOOLEAN` | `true`/`false`| `if_not_exists` | `false`
        DESCRIPTION | `STRING` | `View description` | `description` | `None`
        EXPIRATION TIMESTAMP | `STRING` | `Timestamp` | `expiration_timestamp` | `None`
        FRIENDLY NAME | `STRING` | `Friendly name` | `friendly_name` | `None`
    
    === "Default"
        If the `view_options` `JSON` argument is `NULL`, the default options will correspond to the following DDL statement:

        ```sql
        CREATE OR REPLACE VIEW `[view_id]`
        AS
        [query_statement]
        ```