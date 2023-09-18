## **`PROJECT_IDS`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `PROJECT_IDS`
_**ID**_ | `BQMANAGER.PROJECT_IDS`
_**Description**_ | Configuration function used to determine which `project_ids` are included in the cross-project `INFORMATION_SCHEMA` functions.
_**Type**_ | `USER DEFINED FUNCTION`
_**Arguments**_ | `None`
_**Returns**_ | `project_ids ARRAY<STRING>`
_**Dependencies**_ | `None`

!!! info "configuration: `PROJECT_IDS`"
    
    ```sql
    CREATE OR REPLACE FUNCTION [destination_project_id].BQMANAGER.PROJECT_IDS()
    AS (
    [
    'project_id_1',
    'project_id_2',
    ...
    ]    
    )
    ```

!!! info "execution: `PROJECT_IDS`"
    ```sql
    SELECT [project_id].BQMANAGER.PROJECT_IDS();
    ```