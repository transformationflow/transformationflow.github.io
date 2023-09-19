These objects and functions support profiling of GA4 or Firebase event data.

# Objects
## **`event_profile (JSON)`**
The `event_profile` object is a `JSON` representation of the observed values of `event_names`, `event_params` and `user_properties` in a Firebase or Google Analytics 4 events table.

Field Name | Field Type | Field Description
--- | --- | ---
`event_names` | `ARRAY<STRING>` | All unique observed values for `event_names` in the observation time period.
`event_params`| `ARRAY<STRUCT<key STRING, values <ARRAY<STRUCT<type STRING, count STRING>>>` | Unique observed values for `event_params.key` in the observation time period, with event counts by data type.
`user_properties` | `STRING` | Unique observed values for `user_properties.key` in the observation time period, with event counts by data type.

# Functions
## **`profile_events`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `profile_events`
_**ID**_ | `decodedata-ga4.[region].profile_events`
_**Description**_ | Analyzes date-filtered event data and returns a JSON summary of `event_name`, `event_params` and `user_properties` counts and data types.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `dataset_id STRING, start_date STRING, end_date STRING, OUT event_profile JSON`
_**Returns**_ | `event_profile JSON`
_**Dependencies**_ | `None`

!!! info "execution: `profile_events`"
    === "EU"
        ```sql
        CALL `decodedata-ga4.eu`.profile_events(dataset_id, start_date, end_date, event_profile);
        ```

    === "US"
        ```sql
        CALL `decodedata-ga4.us`.profile_events(dataset_id, start_date, end_date, event_profile);
        ```
