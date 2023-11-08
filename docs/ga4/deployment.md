# Functions

## **`run_ga4_deployment`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `run_ga4_deployment`
_**ID**_ | `decodedata-ga4.[region].run_ga4_deployment`
_**Description**_ | Decoder deployment function for the [Google Analytics 4 BigQuery Export](https://support.google.com/analytics/answer/9358801?hl=en) event data. 
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `ga4_dataset_id STRING, destination_dataset_id STRING, conversion_events ARRAY<STRING>, options JSON`
_**Returns**_ | Creates the date-parameterised table function `[destination_dataset_id].events(start_date DATE, end_date DATE)` and  a custom-generated, editable set of SQL user-defined functions: `add_event_counts`, `decode_event_params` and `decode_user_properties` which are also deployed to the `destination_dataset_id` dataset.
_**Dependencies**_ | `decodedata-ga4-qb`, `bqtools`

!!! info "execution : `run_ga4_deployment`"
    The following command will profile the events in the `ga4_dataset_id` dataset and deploy the functions to the same dataset, with no custom conversion events and without any value exclusions from the `event_names`, `event_params` or `user_properties` output data.

    === "EU"
        ```sql
        CALL `decodedata-ga4.eu.run_ga4_deployment`(ga4_dataset_id, ga4_dataset_id, [], TO_JSON(NULL))
        ```

    === "US"
        ```sql
        CALL `decodedata-ga4.us.run_ga4_deployment`(ga4_dataset_id, ga4_dataset_id, [], TO_JSON(NULL))
        ```

# Execution Sequence
The `project_id` suffix `-qb` denotes query-builder. This is the private code which compiles arguments and logic into SQL statements, which can then be executed or used to create or modify resources. 

```mermaid
sequenceDiagram
    autonumber
    actor bigquery_user
    participant ga4_dataset_id
    bigquery_user ->> decodedata-ga4: CALL: run_ga4_deployment
    decodedata-ga4 ->> decodedata-ga4-qb: BUILD: profile_events
     decodedata-ga4 ->> ga4_dataset_id: EXECUTE: profile_events
    
    ga4_dataset_id ->> decodedata-ga4: RETURN: event_profile.json
    decodedata-ga4 ->> decodedata-ga4-qb: BUILD: add_event_counts
    decodedata-ga4 ->> ga4_dataset_id: DEPLOY: add_event_counts 

    decodedata-ga4 ->> decodedata-ga4-qb: BUILD: decode_event_params
    decodedata-ga4 ->> ga4_dataset_id: DEPLOY: decode_event_params 

    decodedata-ga4 ->> decodedata-ga4-qb: BUILD: decode_user_properties
    decodedata-ga4 ->> ga4_dataset_id: DEPLOY: decode_user_properties 

    decodedata-ga4 ->> ga4_dataset_id: DEPLOY: events(start_date, end_date) 
```

Note that there are additional dependencies on the `bqtools` and `bqtools-qb` libraries to execute common actions such as resource creation and text parsing.