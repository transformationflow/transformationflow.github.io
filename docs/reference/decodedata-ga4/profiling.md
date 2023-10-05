These objects and functions support profiling of GA4 or Firebase event data.

# Objects
## **`event_profile (JSON)`**
The `event_profile` object is a `JSON` representation of the observed values of `event_names`, `event_params` and `user_properties` in a Firebase or Google Analytics 4 events table.

Field Name | Field Type | Field Description
--- | --- | ---
`event_names` | `ARRAY<STRING>` | All unique observed values for `event_names` in the observation time period.
`event_params`| `ARRAY<STRUCT<key STRING, values <ARRAY<STRUCT<type STRING, count STRING>>>` | Unique observed values for `event_params.key` in the observation time period, with event counts by data type.
`user_properties` | `STRING` | Unique observed values for `user_properties.key` in the observation time period, with event counts by data type.

# Tables
## **`EVENT_PROFILES`**
The `EVENT_PROFILES` table is a profile table used to store `event_profile` objects and associated metadata.

Column Name | Data Type | Description
--- | --- | ---
`profile_uid` | STRING | Unique profile ID (JSON hash of all other columns)
`timestamp`	| TIMESTAMP | Profile insert timestamp
`source_resource_id` | STRING |	Specific resource_id profiled (`project_id.dataset_name.table_name`)
`profile_type`	| STRING | Reference for type of profile (`= 'ga4_event_profile'`)
`profile_id`	| STRING | Profile identifier (`= ga4_dataset_id`)
`event_profile`	| JSON | Profile in JSON format

## **`QUERY_PROFILES`**
The `QUERY_PROFILES` table is a profile table used to store `cte_profile` objects and associated metadata.

Column Name | Data Type | Description
--- | --- | ---
`profile_uid` | STRING | Unique profile ID (JSON hash of all other columns)
`timestamp`	| TIMESTAMP | Profile insert timestamp
`source_resource_id` | STRING |	Specific resource_id profiled (`project_id.dataset_name.table_name`)
`profile_type`	| STRING | Reference for type of profile (`= 'cte_profile'`)
`profile_id`	| STRING | Profile identifier (e.g. `'base_ga4_decoder_profile'`)
`event_profile`	| JSON | Profile in JSON format


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

## **`create_event_profile_table`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_event_profile_table`
_**ID**_ | `decodedata-ga4.[region].create_event_profile_table`
_**Description**_ | Creates an `EVENT_PROFILES` table to store `event_profile` objects and associated metadata.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].create_profile_table`

!!! info "execution: `create_event_profile_table`"
    === "EU"
        ```sql
        CALL `decodedata-ga4.eu`.create_event_profile_table(table_id);
        ```

    === "US"
        ```sql
        CALL `decodedata-ga4.us`.create_event_profile_table(table_id);
        ```

## **`create_query_profile_table`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `create_query_profile_table`
_**ID**_ | `decodedata-ga4.[region].create_query_profile_table`
_**Description**_ | Creates an `QUERY_PROFILES` table to store `cte_profile` objects and associated metadata.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools.[region].create_profile_table`

!!! info "execution: `create_query_profile_table`"
    === "EU"
        ```sql
        CALL `decodedata-ga4.eu`.create_query_profile_table(table_id);
        ```

    === "US"
        ```sql
        CALL `decodedata-ga4.us`.create_query_profile_table(table_id);
        ```