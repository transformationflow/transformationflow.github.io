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

# **CREATE Table from DBTF**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_table_from_table_function`
_**ID**_ | `bqtools.[region].create_table_from_table_function`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a single base table as a date partition range from a date-bounded table function, with a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, source_table_function_id STRING, start_date DATE, end_date DATE, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table_from_table_function`, `bqtools-qb.[region].create_table`

# **CREATE View**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_view`
_**ID**_ | `bqtools.[region].create_view`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Creates or replaces a single view defined by an arbitrary sql statement and a set of deployment options.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING, query_statement STRING, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_view`

