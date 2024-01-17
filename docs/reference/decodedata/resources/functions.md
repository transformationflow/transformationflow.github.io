# Deployed Resources

## Overview
The following resources are deployed to the destination dataset in all deployment scenarios.

| Resource Name <div style="width:160px"></div>| Resource Type <div style="width:120px"></div>| Arguments <div style="width:120px"></div>| Resource Description
| --- | --- | --- | -- |
| `GA4_EVENTS` | [`DATE-BOUNDED TABLE FUNCTION`](../../terminology.md) | `start_date DATE`, `end_date DATE` |Base date-bounded table function (DBTF) containing GA4 event-level data with some additional utility columns, data type conversions and decoder function references to include the custom flat `STRUCT` columns `event_count`, `event_param` and `user_property`.
| `GA4_event_names` | [`FUNCTION`](../../terminology.md) | `event_name STRING`  | Returns the custom flat `STRUCT` `event_count` containing sub-columns representing event counts (`1` or `NULL` at event level) for each `event_name` (`event_count.[event_name]`) in addition to total events (`event_count.total_events`) and conversions (`event_count.total_conversions`). These sub-columns can be used as metrics, and [aggregate functions](https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions) can be applied directly to them.
| `GA4_event_parameters` | [`event_params ARRAY<STRUCT>`](https://support.google.com/analytics/answer/7029846?hl=en#zippy=%2Cevent) | [`FUNCTION`](../../terminology.md) | Returns the custom flat `STRUCT` `event_param` comprising sub-columns which contain type-specific values for each `event_params` `key`.
| `GA4_user_properties` | [`user_properties ARRAY<STRUCT>`](https://support.google.com/analytics/answer/7029846?hl=en#zippy=%2Cuser) | [`FUNCTION`](../../terminology.md) | Returns the custom flat `STRUCT` `user_property` comprising sub-columns which contain type-specific values for each `user_properties` `key`.

## Usage
### BigQuery
The `GA4_EVENTS` function can be used in any place you would reference a table in BigQuery, passing the `start_date` and `end_date` arguments to efficiently access a date-bounded subset of the transformed data. Note that using the GoogleSQL [`CURRENT_DATE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#current_date) function enables dynamic ranges to be set in a clear and concise manner:

#### Query Data
??? info "basic query example: `GA4_EVENTS`"
    ***SELECT all data for the past 7 days***
    ```sql
    SELECT * 
    FROM [dataset_id].GA4_EVENTS (
    CURRENT_DATE - 7,
    CURRENT_DATE) 
    ```

??? info "advanced query example: `GA4_EVENTS`"
    ***SELECT all data for the past 7 days, then compute session count per date***
    ```sql
    WITH 
    ga4_events AS (
        SELECT * 
        FROM [dataset_id].GA4_EVENTS (
        CURRENT_DATE - 7,
        CURRENT_DATE)),

    unique_sessions_per_day AS (
        SELECT 
        event_date,
        COUNT(DISTINCT session_id) AS session_id_count
        FROM 
        ga4_events
        GROUP BY
        event_date)

    SELECT *
    FROM unique_sessions_per_day
    ```

#### Create Table
??? info "create output table: `EVENTS`"
    ***CREATE ouptut `EVENTS` table from data between `2016-01-01` and `CURRENT_DATE`, partitioned by `event_date`***
    ```sql
    CREATE OR REPLACE [dataset_id].EVENTS
    PARTITION BY event_date
    AS
    SELECT * 
    FROM [dataset_id].GA4_EVENTS (
    '2016-01-01',
    CURRENT_DATE) 
    ```
    To connect this table optimally to Looker Studio, use the `event_date` partitioning column as the report date field in Looker Studio.

#### Replace Partitions
??? info "replace output table date partitions: `EVENTS`"
    ***REPLACE last 3 days of date partitions in the output `EVENTS` table from the `GA4_EVENTS` table function***
    ```sql
    DECLARE start_date, end_date DATE;

    SET start_date = CURRENT_DATE - 3;
    SET end_date = CURRENT_DATE;

    DELETE 
    FROM [dataset_id].EVENTS
    WHERE event_date 
    BETWEEN start_date AND end_date;

    INSERT 
    INTO [dataset_id].EVENTS
    SELECT * 
    FROM [dataset_id].GA4_EVENTS (start_date, end_date);
    ```
    Using the `start_date`and `end_date` `DATE` variables would enable us to encapsulate this pattern into a [`PROCEDURE`](../../terminology.md) if required, with the `start_date DATE` and `end_date DATE` as arguments. This `PROCEDURE` could then be run periodically using a [`SCHEDULED_QUERY`](../../terminology.md) to keep the data refreshed in a cost-efficient manner.
    
    It might also be desirable to include both of these DDL statements in a single [`TRANSACTION`](https://cloud.google.com/bigquery/docs/transactions) so that an error in either statement would result in a roll-back to the initial state.

### Looker Studio
In order to directly and efficiently connect Looker Studio to the `GA4_EVENTS` date-partitioned table function, we leverage the report date parameters `@DS_START_DATE` and `@DS_END_DATE` in a Custom SQL Query from Looker Studio.  Note that we have to use the GoogleSQL [`PARSE_DATE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#parse_date) function to transform the report date parameters (transmitted in the format `YYYYMMDD`) into SQL-compliant dates (`DATE` formatted, equivalent to the string `YYYY-MM-DD`), which are then passed to the `GA4_EVENTS` table function as the `start_date` and `end_date` parameters).

#### Create Data Source
!!! info "custom query: `GA4_EVENTS`"
    ```sql
    SELECT * 
    FROM [dataset_id].GA4_EVENTS (
    PARSE_DATE ("%Y%m%d", @DS_START_DATE),
    PARSE_DATE ("%Y%m%d", @DS_END_DATE))
    ```





