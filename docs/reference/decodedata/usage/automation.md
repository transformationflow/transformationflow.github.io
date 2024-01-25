# Automation Options
There are several options to ensure that your output table is kept updated in a timely and efficient manner. 

## BigQuery Native
The simplest way to automatically synchronise the output `EVENTS` date-partitioned table is to use the native flow runner (`RUN_FLOW`) function, which is deployed with your core resources by default.

### Automation Logic
The `RUN_FLOW` function takes two arguments: `start_date DATE` and `end_date DATE`, and the behaviour of the function varies depending on the combination of values for each:

Case | `start_date` | `end_date` | Action Description 
--- | --- | --- | --- 
Case 1 | `NULL` | `NULL` | Rebuild Full Table 
Case 2 | `[VALID DATE]` | `[VALID DATE]` | Replace Date Partition Range 
Case 3 | `NULL` | `[VALID DATE]` | Replace All Date Partitions to `end_date` 
Case 4 | `[VALID_DATE]` | `NULL` | Incremental Refresh from `start_date` 

When the deployment function is initially run, a new `EVENTS` table is created as a full rebuild (Case 1), which will contain modelled event data corresponding to all inbound `event_*` data (i.e. the row count will be identical).

#### Case 1: Rebuild
A full table rebuild is executed of the destination `EVENTS` table from the `GA4_EVENTS` date-bounded table function, between the first observed inbound date shard and the `CURRENT_DATE`. Any schema changes require this rebuild to be run. However note that this will query _all_ source data and should therefore be used sparingly to manage costs.

!!! info "`RUN_FLOW`: Rebuild Full Table"
    ```sql
    CALL [dataset_id].RUN_FLOW (NULL, NULL);
    ```

#### Case 2: Range
The destination `EVENTS` table date partitions for a specific date range are replaced with the date partitions returned from the `GA4_EVENTS` date-bounded table function for the same date range. This provides an efficient mechanism for updating and testing logic changes which do not impact the schema.

!!! info "`RUN_FLOW`: Replace Date Partition Range (specified range)"
    ```sql
    CALL [dataset_id].RUN_FLOW ('2024-01-01', '2024-01-15');
    ```

!!! info "`RUN_FLOW`: Replace Date Partition Range (specified start_date to `CURRENT_DATE`)"
    ```sql
    CALL [dataset_id].RUN_FLOW ('2024-01-01', CURRENT_DATE);
    ```

!!! info "`RUN_FLOW`: Replace Date Partition Range (14 days to `CURRENT_DATE`)"
    ```sql
    CALL [dataset_id].RUN_FLOW (CURRENT_DATE - 14, CURRENT_DATE);
    ```

#### Case 3: Start Range
The destination `EVENTS` table date partitions between the first observed inbound date shard and the specified `end_date` are replaced with the partitions returned from the `GA4_EVENTS` date-bounded table function for the same date range. This provides a quick mechanism for replacing a start partition range.

!!! info "`RUN_FLOW`: Replace All Date Partitions to `end_date`"
    ```sql
    CALL [dataset_id].RUN_FLOW (NULL, '2020-12-31');
    ```

!!! info "`RUN_FLOW`: Replace All Date Partitions to `CURRENT_DATE`"
    ```sql
    CALL [dataset_id].RUN_FLOW (NULL, CURRENT_DATE);
    ```

#### Case 4: Incremental
The incremental refresh queries source and destination metadata to identify whether any new date partitions have been received, and inserts new date partitions into the destination `EVENTS` table from the `GA4_EVENTS` date-bounded table function.  If no new date partitions are identified the function does not execute any queries and therefore does not incur costs beyond metadata queries, which are extremely [inexpensive](https://cloud.google.com/bigquery/docs/information-schema-intro#pricing).

Incremental mode is used to control for unpredictable inbound data timing, as they can be run periodically (e.g. every hour or 30 mins) and will execute the incremental refresh _only_ when new source data is identified.

The `start_date` is used to define the observation window in which the new date partitions will be identified.

!!! info "`RUN_FLOW`: Incremental, 7 day observation window"
    ```sql
    CALL [dataset_id].RUN_FLOW (CURRENT_DATE - 7, NULL);
    ```

!!! info "`RUN_FLOW`: Incremental, 14 day observation window"
    ```sql
    CALL [dataset_id].RUN_FLOW (CURRENT_DATE - 14, NULL);
    ```

### Automation Deployment
Native automation is achieved using BigQuery Scheduled Queries, which needs to be enabled for the project. Incremental refresh should be used to minimise costs and control for unpredictable inbound data timing.

It is also recommended to use a query label to support future integration with job-level cost data, enabling granular cost tracking by GA4 property.

