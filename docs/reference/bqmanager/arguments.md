## **`destination_project_id`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `destination_project_id`
_**Data Type**_ | `STRING`
_**Description**_ | The `project_id` containing the `BQMANAGER` dataset used to deploy BigQuery Manager functions.

!!! info "definition: `destination_project_id`"
    ```sql
    SET destination_project_id = 'project_id.dataset_name.table_name'
    ```

## **`source_project_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `source_project_ids`
_**Data Type**_ | `ARRAY<STRING>`
_**Description**_ | The `project_ids` across which the `INFORMATION_SCHEMA` function will aggregate the metadata.

!!! info "definition: `source_project_ids`"
    ```sql
    SET source_project_ids = [
    'project_id_1', 
    'project_id_2', 
    'project_id_3'
    ];
    ```
    
    This variable can also be set from the `PROJECT_IDS` configuration function:
    
    ```sql
    SET source_project_ids = (SELECT [project_id].BQMANAGER.PROJECT_IDS());
    ```

    It can also be passed directly into a function or procedure as a parenthesised argument:
    
    ```sql
    CALL BQMANAGER.EXAMPLE_PROCEDURE (
    destination_project_id,
    (SELECT [project_id].BQMANAGER.PROJECT_IDS()), 
    dataset_batch_size, 
    exclude_dataset_ids
    );
    ```


## **`dataset_batch_size`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `dataset_batch_size`
_**Data Type**_ | `INT64`
_**Description**_ | The number of datasets processed in each batch. It is used to tune the performance as some metadata queries will hit limits and fail if there is a large number of tables in the dataset batch. Setting this to a lower value will result in a more robust but longer-running procedure.

!!! info "definition: `dataset_batch_size`"
    ```sql
    SET dataset_batch_size = 100;
    ```

## **`exclude_dataset_ids`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `exclude_dataset_ids`
_**Data Type**_ | `ARRAY<STRING>`
_**Description**_ | Specific datasets to exclude from the metadata query.  This can be helpful where a large number of tables in a dataset can cause an unrelated metadata query to crash (e.g. an extremely high of table shards in a dataset will cause an `INFORMATION_SCHEMA.PARTITIONS` query on the dataset to fail).

!!! info "definition: `exclude_dataset_ids`"
    ```sql
    SET exclude_dataset_ids = [
    'project_id.dataset_name_1',    
    'project_id.dataset_name_2'
    ];
    ```