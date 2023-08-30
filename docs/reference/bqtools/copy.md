These functions enable copying of BigQuery resources (`EXTERNAL TABLE`, `FUNCTION`, `TABLE FUNCTION`, `PROCEDURE`), maintaining all options from the source resource, and updating all references from the source to the destination dataset.  

# Functions
## **`copy_external_table`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_external_table`
_**Description**_ | Copies a single `EXTERNAL TABLE` from source to destination, maintaining all source table options.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_table_id STRING, destination_table_id STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].copy_external_table`

!!! info "execution: `copy_external_table`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_external_table(source_table_id, destination_table_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_external_table(source_table_id, destination_table_id);
        ```

## **`copy_external_tables`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_external_tables`
_**Description**_ | Copies all `EXTERNAL TABLE` resources from a source dataset to a destination dataset, maintaining all source table options and excluding specific tables if required.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_dataset_id STRING, destination_dataset_id STRING, exclude_table_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].get_external_table_ids`, `bqtools.[region].copy_external_table`

!!! info "execution: `copy_external_tables`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_external_tables(source_dataset_id, destination_dataset_id, exclude_table_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_external_tables(source_dataset_id, destination_dataset_id, exclude_table_ids);
        ```

## **`copy_function`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_function`
_**Description**_ | Copies a single `FUNCTION` from source to destination, maintaining all source table options and updating all references from the source to the destination dataset.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_function_id STRING, destination_function_id STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].copy_function`

!!! info "execution: `copy_function`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_function(source_function_id, destination_function_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_function(source_function_id, destination_function_id);
        ```

## **`copy_functions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_functions`
_**Description**_ | Copies all `FUNCTION` resources from a source dataset to a destination dataset, maintaining all source function options, updating all references from the source to the destination dataset and excluding specific functions if required.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_dataset_id STRING, destination_dataset_id STRING, exclude_function_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].get_function_ids`, `bqtools.[region].copy_function`

!!! info "execution: `copy_functions`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_functions(source_dataset_id, destination_dataset_id, exclude_function_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_functions(source_dataset_id, destination_dataset_id, exclude_function_ids);
        ```

## **`copy_table_function`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_table_function`
_**Description**_ | Copies a single `TABLE FUNCTION` from source to destination, maintaining all source table function options and updating all references from the source to the destination dataset.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_table_function_id STRING, destination_table_function_id STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].copy_table_function`

!!! info "execution: `copy_table_function`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_table_function(source_table_function_id, destination_table_function_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_table_function(source_table_function_id, destination_table_function_id);
        ```

## **`copy_table_functions`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_table_functions`
_**Description**_ | Copies all `TABLE FUNCTION` resources from a source dataset to a destination dataset, maintaining all source table function options, updating all references from the source to the destination dataset and excluding specific functions if required.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_dataset_id STRING, destination_dataset_id STRING, exclude_table_function_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].get_function_ids`, `bqtools.[region].copy_function`

!!! info "execution: `copy_table_functions`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_table_functions(source_dataset_id, destination_dataset_id, exclude_table_function_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_table_functions(source_dataset_id, destination_dataset_id, exclude_table_function_ids);
        ```

## **`copy_procedure`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_procedure`
_**Description**_ | Copies a single `PROCEDURE` from source to destination, maintaining all source table function options and updating all references from the source to the destination dataset.
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `source_procedure_id STRING, destination_procedure_id STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].copy_procedure`

!!! info "execution: `copy_procedure`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_procedure(source_procedure_id, destination_procedure_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_procedure(source_procedure_id, destination_procedure_id);
        ```

## **`copy_procedures`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `copy_procedures`
_**Description**_ | Copies all `PROCEDURE` resources from a source dataset to a destination dataset, maintaining all source procedure options, updating all references from the source to the destination dataset and excluding specific procedures if required.
_**Function Type**_ | `PROCEDURE` 
_**Arguments**_ | `source_dataset_id STRING, destination_dataset_id STRING, exclude_procedure_ids ARRAY<STRING>`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].get_procedure_ids`, `bqtools.[region].copy_procedure`

!!! info "execution: `copy_procedures`"
    === "EU"
        ```sql
        CALL bqtools.eu.copy_procedures(source_dataset_id, destination_dataset_id, exclude_procedure_ids);
        ```

    === "US"
        ```sql
        CALL bqtools.us.copy_procedures(source_dataset_id, destination_dataset_id, exclude_procedure_ids);
        ```