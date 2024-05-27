These utilities package common actions into simple functions.

# **PARSE Dataset ID**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `parse_dataset_id`
_**ID**_ | `bqtools.[region].parse_dataset_id`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Validates a single `dataset_id` and parses constituent elements.
_**Type**_ | `FUNCTION (SQL)`
_**Arguments**_ | `dataset_id STRING`
_**Returns**_ | `STRUCT<dataset_id_input STRING, is_valid BOOL, project_id STRING, dataset_name STRING>`
_**Dependencies**_ | `None`

!!! info "Function Usage" 
    === "EU"
        ```sql
        DECLARE dataset_name STRING;

        SET dataset_id = 'project_id.dataset_name';

        SELECT bqtools.eu.parse_dataset_id(dataset_id);
        ```
    
    === "US"
        ```sql
        DECLARE dataset_name STRING;

        SET dataset_id = 'project_id.dataset_name';

        SELECT bqtools.us.parse_dataset_id(dataset_id);
        ```

# **PARSE Resource ID**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `parse_resource_id`
_**ID**_ | `bqtools.[region].parse_resource_id`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Validates a single `resource_id` and parses constituent elements.
_**Type**_ | `FUNCTION (SQL)`
_**Arguments**_ | `resource_id STRING`
_**Returns**_ | `STRUCT<resource_id_input STRING, is_valid BOOL, project_id STRING, dataset_name STRING, resource_name STRING, dataset_id STRING, resource_id STRING>`
_**Dependencies**_ | `None`

!!! info "Function Usage" 
    === "EU"
        ```sql
        DECLARE resource_id STRING;

        SET resource_id = 'project_id.dataset_name.resource_name';

        SELECT bqtools.eu.parse_resource_id(resource_id);
        ```
    
    === "US"
        ```sql
        DECLARE resource_id STRING;

        SET resource_id = 'project_id.dataset_name.resource_name';

        SELECT bqtools.us.parse_resource_id(resource_id);
        ```