The following definitions are used consistently across all functions and development activity.  These definitions may override some of the standard Google definitions which can be inconsistent across different system functions, views and user interfaces.

# Resource Types
Resource Type <div style="width:80px"></div> | Description | Docs <div style="width:80px"></div>
--- | --- | ---
`TABLE` | The collective terminology for a resource of type `BASE TABLE`, `PARTITIONED TABLE`, `SHARDED TABLE`, `EXTERNAL TABLE`, `SNAPSHOT` or `VIEW` | [Introduction to tables](https://cloud.google.com/bigquery/docs/tables-intro)
`BASE TABLE` | A native BigQuery table, for which the data is physically stored within BigQuery | [Standard BigQuery tables](https://cloud.google.com/bigquery/docs/tables-intro#standard_tables)
`PARTITIONED TABLE` | A native BigQuery table where the data is stored in physically separate partitions (typically using a `DATE` column), enabling efficient and performant query execution | [Partitioned tables](https://cloud.google.com/bigquery/docs/partitioned-tables)
`DATE-PARTITIONED TABLE (DPT)` | A partitioned table which is partitioned by a specific date column. | [Partitioned tables](https://cloud.google.com/bigquery/docs/partitioned-tables)
`SHARDED TABLE` | A set of tables with a common schema and prefix, but with distinct suffixes (e.g. `[prefix]_YYYYMMDD` for date-based sharding) | [Partitioning versus sharding](https://cloud.google.com/bigquery/docs/partitioned-tables#dt_partition_shard)
`DATE-SHARDED TABLE (DST)` | A sharded table which is sharded by date, with table names in the format `[prefix]_YYYYMMDD` | [Partitioning versus sharding](https://cloud.google.com/bigquery/docs/partitioned-tables#dt_partition_shard)
`EXTERNAL TABLE` | A BigQuery table where the data is stored externally, either within the Google ecosystem (Google Sheets, files on Google Cloud Storage) or externally (Amazon S3, Azure Blob Storage) | [External tables](https://cloud.google.com/bigquery/docs/tables-intro#external_tables)
`SNAPSHOT` | An efficient mechanism for recording point-in-time table contents | [Introduction to table snapshots](https://cloud.google.com/bigquery/docs/table-snapshots-intro)
`VIEW` | An ephemeral table-like logical resource defined by a SQL query | [Views](https://cloud.google.com/bigquery/docs/tables-intro#views)
`ROUTINE` | The collective terminology for a resource of type `FUNCTION`, `PROCEDURE`, `TABLE FUNCTION` or `REMOTE FUNCTION` | [Manage Routines](https://cloud.google.com/bigquery/docs/routines)
`FUNCTION` | User-defined code (SQL, native Javascript or packaged Javascript libraries) which wraps logic into a reusable and shareable function, taking zero or more type-specific arguments and returning a single value | [User-defined functions](https://cloud.google.com/bigquery/docs/user-defined-functions)
`PROCEDURE` | A set of statements which takes zero or more type-specific arguments and enables complex logic and actions to be packaged into a single, reusable and shareable resource | [SQL stored procedures](https://cloud.google.com/bigquery/docs/procedures)
`TABLE FUNCTION (TF)` | A parameterized `VIEW` which can support more complex, optimized query patterns | [Table functions](https://cloud.google.com/bigquery/docs/table-functions)
`DATE-BOUNDED TABLE FUNCTION (DBTF)` | A table function with precisely two `DATE` parameters as arguments: `start_date` and ` end_date` | [Table functions](https://cloud.google.com/bigquery/docs/table-functions)
`REMOTE FUNCTION` | A [Cloud Function](https://cloud.google.com/functions/) written in one of a variety of languages which can be called like a BigQuery `FUNCTION`, enabling interaction with external APIs and libraries from BigQuery queries and workflows | [Work with remote functions](https://cloud.google.com/bigquery/docs/remote-functions)
`SCHEDULED QUERY` | An arbitrary SQL statement which can be scheduled to run periodically | [Scheduling Queries](https://cloud.google.com/bigquery/docs/scheduling-queries)

# Naming Conventions
Term <div style="width:110px"></div> | Data Type | Description 
--- | --- | --- 
`project_id` | `STRING` | The globally unique identifier for each [project](https://cloud.google.com/resource-manager/docs/creating-managing-projects)
`dataset_name` | `STRING` | The name for each [dataset](https://cloud.google.com/bigquery/docs/datasets), unique in each project
`dataset_id` | `STRING` | The globally unique identifier for each dataset, composed as `project_id.dataset_name`
`resource_name`  | `STRING` | The single name for each resource (`TABLE`, `VIEW`, `FUNCTION`, `TABLE FUNCTION` or `PROCEDURE`)
`resource_id` | `STRING` | The globally unique identifier for an individual `TABLE`, `VIEW`, `FUNCTION`, `TABLE FUNCTION` or `PROCEDURE`, composed as `project_id.dataset_name.resource_name`
`date_id` | `STRING` | A `STRING` representantion of a date in the format `YYYYMMDD`
`shard_id` | `STRING` |  A `STRING` representantion of a table shard date in the format `YYYYMMDD`
`shard_date` | `DATE` |  A `DATE` representantion of a table shard date
`partition_id` | `STRING` |  A `STRING` representantion of a table partition date in the format `YYYYMMDD`
`partition_date` | `DATE` |  A `DATE` representantion of a table partition date


# Language Types
Language Type <div style="width:80"></div> | Description | Docs <div style="width:80px"></div>
--- | --- | ---
GoogleSQL | An ANSI compliant superset of Structured Query Language ([SQL](https://en.wikipedia.org/wiki/SQL))| [SQL in BigQuery](https://cloud.google.com/bigquery/docs/introduction-sql)
Procedural Language | The core language which enables programmatic execution of arbitrary logic | [Procedural Language](https://cloud.google.com/bigquery/docs/reference/standard-sql/procedural-language)
Data Definition Language | Statements which enable creation and modification of BigQuery resources | [DDL Statements](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language)
Data Manipulation Language | Statements which enable insertion, deletion and modification of data | [DML Statements](https://cloud.google.com/bigquery/docs/reference/standard-sql/dml-syntax)
Data Control Language | Statements which enable access control to BigQuery resources | [DCL Statements](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-control-language)