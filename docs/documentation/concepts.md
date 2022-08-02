## Table Types
The different table types in BigQuery are the building blocks of any transformation flow, and understanding the differences is critical to building robust and efficient flows.

### Tables
Tables are the base-level building blocks of BigQuery, however they are the least used in transformation flows.  Tables have a specific schema and you load data directly into them, however partitioned tables are preferred in most situations. 

### External Tables (Storage) 
External tables enable direct querying of data files in Google Cloud Storage (GCS) buckets, as well as AWS S3 or Azure Storage buckets.  There are some complexitites and limitations based on e.g. folder structure and file naming, however they can be used to build powerful, robust data ingestion points for your transformation flows. 

They also add redundancy and resilience to your data flows as deleting the external table does not delete the underlying data. 

### "External Tables (Google Sheets)
External tables can also be created from sheets in a Google Sheets workbook, which is a powerful mechanism but not without potential pitfalls.  They are very useful as e.g. match tables or for semi-automated QA based on point-in-time data extraction from another non-integrated system.  However some potential challenges are:

1. Schema Detection does not work when all columns are detected as a string type.  
2. Downstream users and systems will require federated access to the sheets.

Point 1 can be addressed by adding an incremental integer `row_id` into the sheet and point 2 by creating a related flow stage (with the same `flow_stage_title` but an incremented `flow_stage_index`) as a table.  

They can also be used as temporary data ingestion points (if an external system can output to Google Sheets), however this is not recommended as a long-term approach as you can quickly hit row limits and performance challenges.

### Views
Views are the foundational building block of transformation flows and are the mechanism by which all transformation/integration logic is written in pure SQL, as well as the functions in the `flowfunctions` library. 


### Partitioned Tables
Partitioned tables are the foundation for efficient data transformation flows.  In partitioned tables, the data is actually physically stored in sequential partitions, meaning that if you know the partition(s) you are looking for it is quick and cost-effective to access that data.  They eliminate the need for expensive and slow full-table-scans and help avoid unnecessarily processing data on a repeating basis. 

They also enable optimised downstream performance in BI tools such as Data Studio to to efficient querying.  If you set the date parameters in a Google Data Studio report to be a date-partitioned field, then the data transfer between BigQuery and Data Studio is limited just to the partitions required.  This increases responsiveness, reduces the volume of data queried (and associated cost) and improves the end-user experience. 

Data Quality Assurance (QA) is also optimised as spot checks can be done on specfic partitions without full table scans.

### Date Sharded Tables
Date sharded tables are similar to date partitioned tables, but with some key differences.  They are actually separate tables with a common `table_name_stem_` and a date suffix in the format `YYYYMMDD`.  In the user interface they appear to be a single table with a dropdown to select the date.

This means that the individual shards can be scanned without requiring a full table scan, using `_TABLE_SUFFIX` pseudocolumn and a wildcard in the query.  You can limit the shards scanned using a number of different SQL constructs:

=== "="
    ```sql
    SELECT *
    FROM `project_id.firebase_dataset.events_*`
    WHERE _TABLE_SUFFIX = '20220701'
    ```

=== ">="
    ```sql
    SELECT *
    FROM `project_id.firebase_dataset.events_*`
    WHERE _TABLE_SUFFIX >= '20220701'
    ```

=== "BETWEEN"
    ```sql
    SELECT *
    FROM `project_id.firebase_dataset.events_*`
    WHERE _TABLE_SUFFIX BETWEEN '20220701' AND '20220731'
    ```

This is the mechanism we use to limit the data scanned every time the flow is executed, so instead of reprocessing _all_ data every day, only the incremental data is updated.  