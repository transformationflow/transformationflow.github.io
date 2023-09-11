This function profiles GA4 or Firebase event data.

# Functions
## **`profile_events`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `profile_events`
_**ID**_ | `decodedata-ga4.[region].profile_events`
_**Description**_ | Analyzes date-filtered event data and returns a JSON summary of `event_name`, `event_param` and `user_property` counts and data types.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `(dataset_id STRING, start_date STRING, end_date STRING, OUT event_profile JSON)`
_**Returns**_ | `event_profile JSON`
_**Dependencies**_ | `None`

!!! info "execution: `profile_events`"
    === "EU"
        ```sql
        CALL bqtools.eu.profile_events(dataset_id, start_date, end_date, event_profile);
        ```

    === "US"
        ```sql
        CALL bqtools.us.profile_events(dataset_id, start_date, end_date, event_profile);
        ```
