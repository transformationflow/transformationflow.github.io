If your data, table schemas, views and routines are safely backed up and something _does_ go awry, there are just a few simple steps required to recover all of your resources.

# Inbound Data Recovery
The functions used to restore inbound data depends on the source and the backup types.

## Firebase from BigQuery
Restoring from a Firebase table backup to a new dataset is exactly the same as copying the table to a new dataset.  However, due to the sharded nature of Firebase tables, you cannot simply use a `CREATE OR REPLACE TABLE`statement and you have to copy the data one shard at a time.  

This is achieved by calling the following function:

=== "region-us" 
    ```sql
    CALL flowfunctions.copy.copy_firebase_table (
        source_dataset_ref, -- STRING 
        destination_dataset_ref -- STRING
        )
    ```

=== "region-eu" 
    ```sql
    CALL flowfunctionseu.copy.copy_firebase_table (
        source_dataset_ref, -- STRING 
        destination_dataset_ref -- STRING
        )
    ```

## Firebase from GCS
Restoring from a GCS backup is a more complex multi-stage process, but this complexity is abstracted away by the internal function operations.  To execute restore this data to a destination dataset, use the following function:

=== "region-us" 
    ```sql
    CALL flowfunctions.restore.restore_firebase_table_from_gcs_backup (
        backup_firebase_gcs_bucket_name, -- STRING
        restore_dataset_ref -- STRING 
        )
    ```

=== "region-eu" 
    ```sql
    CALL flowfunctionseu.restore.restore_firebase_table_from_gcs_backup (
        backup_firebase_gcs_bucket_name, -- STRING
        restore_dataset_ref -- STRING 
        )
    ```

## Flow Resources from DDL
When you regularly schedule the `flowfunctions.backup` functions, you always have a daily snapshot of your transformation flow resources, which makes it very simple to restore your flow logic to any point in time.  This is useful to restore the end-to-end flow if it is accidentally deleted or corrupted, but can also be used to 'time travel' if you want to identify when a particular result or behaviour first arose.

### Routines
Since views often use `ROUTINES`, it is often preferable to restore the routines before the views and tables.  This is achieved by executing the following function:

=== "region-us" 
    ```sql
    CALL flowfunctions.restore.restore_routines_from_info_schema_backup(
        info_schema_routines_table_ref, -- STRING
        source_routines_dataset_name, -- STRING
        destination_dataset_ref -- STRING
        );
    ```

=== "region-eu" 
    ```sql
    CALL flowfunctionseu.restore.restore_routines_from_info_schema_backup(
        info_schema_routines_table_ref, -- STRING
        source_routines_dataset_name, -- STRING
        destination_dataset_ref -- STRING
        );
    ```

The `source_routines_dataset_name` means that you have to restore routines from one source dataset at a time.

### Tables and Views
Transformation flow stage naming conventions mean that flow stages can _only_ have dependencies on stages with a lower `flow_stage_index`, which means that a simple sort on `table_name` and executing the DDL in order ensures that none of these restore DDL statements will fail due to dependencies not yet existing. This resource restoration is achieved using the following function:

=== "region-us" 
    ```sql
    CALL flowfunctions.restore.restore_tables_and_views_from_info_schema_backup(
        info_schema_tables_table_ref, -- STRING
        source_dataset_name, -- STRING
        destination_dataset_ref -- STRING
        );
    ```

=== "region-eu" 
    ```sql
    CALL flowfunctionseu.restore.restore_tables_and_views_from_info_schema_backup(
        info_schema_tables_table_ref, -- STRING
        source_dataset_name, -- STRING
        destination_dataset_ref -- STRING
        );
    ```
