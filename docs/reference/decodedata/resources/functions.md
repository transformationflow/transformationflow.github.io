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
The `GA4_EVENTS` function can then be used in any place you would reference a table in BigQuery, passing the `start_date` and `end_date` arguments to efficiently access a date-bounded subset of the transformed data. Note that using the GoogleSQL [`CURRENT_DATE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#current_date) function enables dynamic ranges to be set in a clear and concise manner:

!!! info "basic query example: `GA4_EVENTS`"
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

### Looker Studio
In order to directly and efficiently connect Looker Studio to the `GA4_EVENTS` date-partitioned table function, we leverage the report date parameters `@DS_START_DATE` and `@DS_END_DATE` in a Custom SQL Query from Looker Studio.  Note that we have to use the GoogleSQL [`PARSE_DATE`](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#parse_date) function to transform the report date parameters (transmitted in the format `YYYYMMDD`) into SQL-compliant dates (`DATE` formatted, equivalent to the string `YYYY-MM-DD`), which are then passed to the `GA4_EVENTS` table function as the `start_date` and `end_date` parameters).

!!! info "custom query: `GA4_EVENTS`"
    ```sql
    SELECT * 
    FROM [dataset_id].GA4_EVENTS (
    PARSE_DATE ("%Y%m%d", @DS_START_DATE),
    PARSE_DATE ("%Y%m%d", @DS_END_DATE))
    ```





