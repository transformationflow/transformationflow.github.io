Whether interacting with your data in BigQuery via the Console, API or scripted SQL statements, it _can_ happen that the wrong table or dataset is deleted by accident.  Whilst in some instances this can be recovered (e.g. using [Time Travel](https://cloud.google.com/bigquery/docs/time-travel)), it is good practice to backup your data and code in a manner which enables quick recovery in case something goes wrong.

# Inbound Data Backup
If your data is coming into BigQuery via native Google services like Firebase or Google Analytics, it is streaming into BigQuery and is not necessarily stored anywhere else.  This means that without backing up your data, accidental deletion of the dataset might be a non-recoverable action.

## Backup Destinations
There are a number of different options for where to store your data backups, however they are all relatively simple to execute, schedule and customize.  Live functions are currently available for backing up to:

- BigQuery Table (in different Dataset)
- Google Cloud Storage Bucket (Parquet, Uncompressed)
- Google Cloud Storage Bucket (JSON)

Parquet is the preferred file format due to the fact that the schema is included in the file header, which makes recovery more reliable.

### BigQuery
BigQuery backups check for the existence of specific partitions and shards in the backup table, and then insert any data which has not yet been backed up into the table.  This is done via the table metadata, so does not require full table scans.

### Google Cloud Storage
GCS backups write into a single folder for each shard, using a [wildcard](https://cloud.google.com/bigquery/docs/exporting-data#exporting_data_into_one_or_more_files) to split the data into different files in case the shard size is more than the 1GB limit.

## Source Backups
### Firebase
Firebase data comes directly into BigQuery on a daily basis as shards, which can be efficiently backed up to either GCS or BigQuery.  

#### Firebase to BigQuery
This function checks for the existence of date shards in the `destination_dataset_ref`, and inserts the data when a new shard is detected in the source data.  This queries table metadata, so can be run on a regular basis (e.g. hourly) and incurs only negligible cost.

=== "us" 
    ```sql
    CALL flowfunctions.backup.backup_firebase_table (
            source_dataset_ref, -- STRING
            destination_dataset_ref -- STRING
            );
    ```
=== "eu" 
    ```sql
    CALL flowfunctionseu.backup.backup_firebase_table (
            source_dataset_ref, -- STRING
            destination_dataset_ref -- STRING
            );
    ```

#### Firebase to GCS
This function backs up table shards from the `source_dataset_ref` into different files including the date in the format `YYYYMMDD` in the destination bucket determined by `export_gcs_bucket_name`. The offset parameters `start_days_offset` and `end_days_offset` determine how many days to export (offset from today as defined by the `CURRENT_DATE()` in SQL i.e. 0 = today).

=== "us" 
    ```sql
    CALL flowfunctions.backup.backup_firebase_table_to_gcs (
            source_dataset_ref, -- STRING
            export_gcs_bucket_name, -- STRING
            start_days_offset, -- INT64
            end_days_offset -- INT64
            );
    ```
=== "eu" 
    ```sql
    CALL flowfunctionseu.backup.backup_firebase_table_to_gcs (
            source_dataset_ref, -- STRING
            export_gcs_bucket_name, -- STRING
            start_days_offset, -- INT64
            end_days_offset -- INT64
            );
    ```
=== "example" 
    ```sql
    CALL flowfunctions.backup.backup_firebase_table_to_gcs (
            'project_id.dataset_name.table_name', 
            'my-gcs-bucket', 1, 3)
    ```

The example will export table shards from the `project_id.dataset_name.table_name` table into files (with the date included in the format `YYYYMMDD`) in the `gs://my-gcs-bucket` GCS bucket for days between yesterday and three days ago, overwriting any files which already exist.  For example, a single file on 2022-07-01 would be saved as `gs://my-gcs-bucket/events_20220701_00000000.parquet`, and if the data to export was larger than 1GB, the second filename would be `gs://my-gcs-bucket/events_20220701_00000001.parquet`.  

# DDL-Based Backups
Data Definition Language (DDL) backups leverage the fact that the DDL to recreate resources is available via the `INFORMATION_SCHEMA`.  This means that backing up the DDL into a BigQuery dataset gives us the mechanism to recover any views, external tables or routines.

The following functions will backup all view, routine and external tables DDL to a set of date-sharded table in the destination dataset defined by `destination_dataset_ref`.  Scheduling this statement on a daily basis will give you a daily backup of all DDL in the datasets defined in `backup_dataset_refs`.

## All Resources
=== "us" 
    ```sql
    CALL flowfunctions.backup.backup_resource_metadata (
            backup_dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```
=== "eu" 
    ```sql
    CALL flowfunctionseu.backup.backup_resource_metadata (
            backup_dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```

Individual resource types can also be backed up using the following sub-functions.

## Routines
=== "us" 
    ```sql
    CALL flowfunctions.backup.backup_info_schema_routines (
            dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```
=== "eu" 
    ```sql
    CALL flowfunctionseu.backup.backup_info_schema_routines (
            dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```

## Views
=== "us" 
    ```sql
    CALL flowfunctions.backup.backup_info_schema_views (
            dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```
=== "eu" 
    ```sql
    CALL flowfunctionseu.backup.backup_info_schema_views (
            dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```

## Tables
=== "us" 
    ```sql
    CALL flowfunctions.backup.backup_info_schema_tables (
            dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```
=== "eu" 
    ```sql
    CALL flowfunctionseu.backup.backup_info_schema_tables (
            dataset_refs, -- ARRAY<STRING>
            destination_dataset_ref -- STRING
            );
    ```

Note that when recovering tables, they will be created as empty tables with the exact same schema as the source table.  In the context of a Transformation Flow this is not problematic as the tables will be filled with data when the flow is run and the inbound data should be backed up separately.

# Data Recovery
Once your backups are tested and scheduled, hopefully you will never have to touch or use them again!  However, in the eventuality that you do need to restore these tables, there are a few different approaches depending on 

## Inbound Data Recovery
The functions used to restore inbound data depends on the source and the backup types.

### Restore Firebase Backup from BigQuery
Restoring from a Firebase table backup to a new dataset is exactly the same as copying the table to a new dataset.  However, due to the sharded nature of Firebase tables, you cannot simply use a `CREATE OR REPLACE TABLE`statement and you have to copy the data one shard at a time.  

This is achieved by calling the following function:

=== "us" 
    ```sql
    CALL flowfunctions.copy.copy_firebase_table (
        source_dataset_ref, -- STRING 
        destination_dataset_ref -- STRING
        )
    ```

=== "eu" 
    ```sql
    CALL flowfunctionseu.copy.copy_firebase_table (
        source_dataset_ref, -- STRING 
        destination_dataset_ref -- STRING
        )
    ```

### Restore Firebase Backup from GCS
Restoring from a GCS backup is a more complex multi-stage process, but this complexity is abstracted away by the internal function operations.  To execute restore this data to a destination dataset, use the following function:

=== "us" 
    ```sql
    CALL flowfunctions.restore.restore_firebase_table_from_gcs_backup (
        backup_firebase_gcs_bucket_name, -- STRING
        restore_dataset_ref -- STRING 
        )
    ```

=== "eu" 
    ```sql
    CALL flowfunctionseu.restore.restore_firebase_table_from_gcs_backup (
        backup_firebase_gcs_bucket_name, -- STRING
        restore_dataset_ref -- STRING 
        )
    ```

### Restore Flow Resources from DDL
Flow resource restoration (Routines, Views, Tables) is executed via the following function:

=== "us" 
    ```sql
    CALL flowfunctions.restore.restore_resources_from_info_schema_backup (
        info_schema_backup_table_ref, -- STRING
        restore_dataset_suffix -- STRING 
        )
    ```

=== "eu" 
    ```sql
    CALL flowfunctionseu.restore.restore_resources_from_info_schema_backup (
        info_schema_backup_table_ref, -- STRING
        restore_dataset_suffix -- STRING 
        )
    ```

The `restore_dataset_suffix` will add a suffix to each dataset in the backup DDL.