The `BQMANAGER` library adds additional functionality to BigQuery to support common monitoring, management and optimisation activities and patterns. They also enable BigQuery administrators to maintain a record of job logs beyond the standard six month period (the default, inextensible default).

# Functions
Once deployed to a client project, all user-facing functions, tables and table functions are found in the `[project_id].BQMANAGER` dataset.

## **`TABLES`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `TABLES`
_**ID**_ | `BQMANAGER.TABLES`
_**Description**_ | A simplified summary of all `BASE TABLE`, `EXTERNAL TABLE` and `VIEW` resources, including flags for partitioned and sharded tables, external table formats, row counts and table size in `MB`, `GB` and `TB`.
_**Type**_ | `VIEW`
_**Configuration**_ | The `project_ids` to include in the source tables are set in the `BQMANAGER.PROJECT_IDS` function.
_**Dependencies**_ | `BQMANAGER.INFORMATION_SCHEMA_TABLES`, `BQMANAGER.INFORMATION_SCHEMA_TABLES_METADATA`, `BQMANAGER.INFORMATION_SCHEMA_PARTITIONS`, `BQMANAGER.INFORMATION_SCHEMA_TABLE_OPTIONS`.

## **`GET_SCHEDULED_QUERY_LOGS()`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `GET_SCHEDULED_QUERY_LOGS`
_**ID**_ | `BQMANAGER.GET_SCHEDULED_QUERY_LOGS`
_**Description**_ | Logs the status of each specified Scheduled Query execution, with `data` and `attributes` JSON payloads decoded into simple columns.
_**Type**_ | `TABLE FUNCTION`
_**Arguments**_ | `None`
_**Configuration**_ | To capture logs from a Scheduled Query, the query notification cloud pub/sub topic needs to be set to `projects/[project_id]/topics/BQMANAGER.SCHEDULED_QUERY_LOGS`.
_**Dependencies**_ | PubSub Topic: `projects/[project_id]/topics/BQMANAGER.SCHEDULED_QUERY_LOGS`, Log Table: `BQMANAGER.SCHEDULED_QUERY_LOGS`.

## **`INFORMATION_SCHEMA_JOBS`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `INFORMATION_SCHEMA_JOBS`
_**ID**_ | `BQMANAGER.INFORMATION_SCHEMA_JOBS`
_**Description**_ | An aggregated table of all raw job logs across multiple `project_ids`.
_**Type**_ | `TABLE`
_**Partitioning**_ | `DATE(creation_time)`
_**Configuration**_ | The `project_ids` to include in the source tables are set in the `BQMANAGER.PROJECT_IDS` function.
_**Dependencies**_ | `bqtools.[region].INFORMATION_SCHEMA_JOBS`


# Data Refresh
In order to refresh the data in the underlying raw tables, the following functions are used.

## **`REFRESH_INFORMATION_SCHEMAS()`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `REFRESH_INFORMATION_SCHEMAS`
_**ID**_ | `BQMANAGER.REFRESH_INFORMATION_SCHEMAS`
_**Description**_ | Refresh all of the underlying raw `INFORMATION_SCHEMA` metadata tables (excluding `JOBS`).
_**Type**_ | `PROCEDURE`
_**Configuration**_ | Configuration is set in the `CONFIGURATION` section of the script. The `project_ids` to include in the source tables are set in the `BQMANAGER.PROJECT_IDS` function.
_**Dependencies**_ | `bqtools.[region].INFORMATION_SCHEMA_JOBS`
_**Automation**_ | The recommended scheduled query name is `BQMANAGER.REFRESH_INFORMATION_SCHEMAS`.  Adding query labels to the scheduled query using `SET @@query_label = "scheduled_query_id:bqmanager_refresh_information_schemas"` will enable the precise job logs to be attributed to specific scheduled queries. 
_**Cost**_ | This is an aggregate metadata query, but the scale of the processed/billed data will vary based on the number of projects and datasets.  As such it is recommended to test and set a schedule based on specific needs (e.g. weekly then on-demand when conducting analysis).

## **`REFRESH_INFORMATION_SCHEMA_JOBS()`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `REFRESH_INFORMATION_SCHEMA_JOBS`
_**ID**_ | `BQMANAGER.REFRESH_INFORMATION_SCHEMA_JOBS`
_**Description**_ | Replace the (configurable) current and last day data with `INFORMATION_SCHEMA.JOBS` logs across multiple projects.
_**Type**_ | `PROCEDURE`
_**Configuration**_ | Configuration is set in the `CONFIGURATION` section of the script. To 
_**Dependencies**_ | `bqtools.[region].INFORMATION_SCHEMA_JOBS`
_**Automation**_ | The recommended scheduled query name is `BQMANAGER.REFRESH_INFORMATION_SCHEMA_JOBS`.  Adding query labels to the scheduled query using `SET @@query_label = "scheduled_query_id:bqmanager_refresh_information_schema_jobs"` will enable the precise job logs to be attributed to specific scheduled queries. 
_**Cost**_ | By default this overwrites the current and past day of log data, so it is recommended to run on a daily schedule.  Costs depend on the scale of usage but should not be significant.