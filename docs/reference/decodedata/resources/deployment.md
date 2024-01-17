# Deployment Standards
## Arguments
The following arguments are used in various combinations when executing the `decodedata` deployment functions.

Argument <div style="width:160px"></div>| Data Type <div style="width:120px"></div>| Description
--- | --- | ---
`ga4_dataset_id` | `STRING` | The `dataset_id` of the dataset containing the GA4 BigQuery data export.
`destination_dataset_id` | `STRING` | The `dataset_id` of the dataset into which the decoder functions will be deployed. Use the same value as `ga4_dataset_id` to deploy to the same dataset as the inbound GA4 data (which is recommended, especially when deploying across multiple properties).
`conversion_events` | `ARRAY<STRING>` | The `event_name` values to be marked as conversion events.
`event_options` | `JSON` | Options to add specific include/exclude logic for custom profiling of event names, parameters and user properties. Accepts `NULL` for default logic.
`ga4_dataset_ids` | `ARRAY<STRING>` | The `ga4_dataset_id` value corresponding to datasets containing GA4 BigQuery data exports.
`integrated_dataset_id` | `STRING` | The `dataset_id` of the dataset into which the integrated output resources will be deployed.
 
## Permissions
The logged-in user requires the following permissions to execute the deployment functions on user GA4 data:

| Permission | Permitted Dataset
| --- | --- 
| `bigquery.routines.get` | `decodedata.[region]`
| `bigquery.jobs.create` | `project_id`
| `bigquery.tables.getData` | `project_id.ga4_dataset_id`
| `bigquery.routines.create` | `project_id.destination_dataset_id`

# Deployment Functions

## **`GA4_STANDARD`**
| Deployment Function <div style="width:180px"></div>| App or Web | Single or Multi-Property | Event Names | Event Params | User Properties | Aligned Schema
| --- | :-: | :-: | :-: | :-: | :-: | :-: |
| `DEPLOY_GA4_STANDARD_WEB` | Web | Single | Standard | Standard | None | :material-check:
| `DEPLOY_GA4_STANDARD_APP` | App | Single | Standard | Standard | None | :material-check:

These functions deploy the decoder functions into the destination dataset, using the set of event names defined by the [GA4 Automatically Collected Events](https://support.google.com/analytics/answer/9234069?hl=en&ref_topic=13367566&sjid=7618755317365101827-EU) specification and event parameters defined by the [GA4 Enhanced Event Measurement](https://support.google.com/analytics/answer/9216061?hl=en&ref_topic=13367566&sjid=7618755317365101827-EU) specification for Web or App.  Conversion events are aligned to the [GA4 Default Conversions](https://support.google.com/analytics/answer/13128484?sjid=352449609507277522-EU) for Web or App. No user properties are included.

Since the output schema will be consistent for _all_ GA4 properties, this approach allows downstream data integration of Web _or_ App properties without additional transformation. Since Web and App properties will have different schemas, they would require additional transformation to integrate Web and App data.

!!! info "deployment: `DEPLOY_GA4_STANDARD_WEB`"
    === "EU"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_STANDARD_WEB (ga4_dataset_id, destination_dataset_id);
        ```

    === "US"
        ```sql
        CALL decodedata.us.DEPLOY_GA4_STANDARD_WEB (ga4_dataset_id, destination_dataset_id);
        ```

!!! info "deployment: `DEPLOY_GA4_STANDARD_APP`"
    === "EU"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_STANDARD_APP (ga4_dataset_id, destination_dataset_id);
        ```

    === "US"
        ```sql
        CALL decodedata.us.DEPLOY_GA4_STANDARD_APP (ga4_dataset_id, destination_dataset_id);
        ```

## **`GA4_CUSTOM`**
| Deployment Function <div style="width:180px"></div>| App or Web | Single or Multi-Property | Event Names | Event Params | User Properties | Aligned Schema
| --- | :-: | :-: | :-: | :-: | :-: | :-: |
| `DEPLOY_GA4_CUSTOM_WEB` | Web | Single | Observed + Standard | Observed + Standard | Profiled | :material-close:
| `DEPLOY_GA4_CUSTOM_APP` | App | Single | Observed + Standard | Observed + Standard | Profiled | :material-close:

These functions deploys the decoder functions into the destination dataset, using the combination of observed event names and the standard set of event names defined by the [GA4 Automatically Collected Events](https://support.google.com/analytics/answer/9234069?hl=en&ref_topic=13367566&sjid=7618755317365101827-EU) specification for Web or App.  Event parameters are a combination of observed event parameters and event parameters defined by the [GA4 Enhanced Event Measurement](https://support.google.com/analytics/answer/9216061?hl=en&ref_topic=13367566&sjid=7618755317365101827-EU) specification for Web or App, and user properties are defined by the values observed in the data . 

Since the output schema will be bespoke for each GA4 properties, this approach will typically not allow downstream data integration without additional transformation.

!!! info "deployment: `DEPLOY_GA4_CUSTOM_WEB`"
    === "EU"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_CUSTOM_WEB (ga4_dataset_id, destination_dataset_id, conversion_events, event_options);
        ```

    === "US"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_CUSTOM_WEB (ga4_dataset_id, destination_dataset_id, conversion_events, event_options);
        ```

!!! info "deployment: `DEPLOY_GA4_CUSTOM_APP`"
    === "EU"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_CUSTOM_APP (ga4_dataset_id, destination_dataset_id, conversion_events, event_options);
        ```

    === "US"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_CUSTOM_APP (ga4_dataset_id, destination_dataset_id, conversion_events, event_options);
        ```

## **`GA4_CUSTOM_INTEGRATED`**
| Deployment Function <div style="width:180px"></div>| App or Web | Single or Multi-Property | Event Names | Event Params | User Properties | Aligned Schema
| --- | :-: | :-: | :-: | :-: | :-: | :-:
| `GA4_CUSTOM_WEB_INTEGRATED` | Web | Multi | Observed + Standard | Observed + Standard | Observed | :material-check:
| `GA4_CUSTOM_APP_INTEGRATED` | App | Multi | Observed + Standard | Observed + Standard | Observed | :material-check:

This function deploys the decoder functions into _each_ GA4 dataset, using the combination of observed event names across _all_ specified GA4 properties and the standard set of event names defined by the [GA4 Automatically Collected Events](https://support.google.com/analytics/answer/9234069?hl=en&ref_topic=13367566&sjid=7618755317365101827-EU) specification for Web or App.  Event parameters are a combination of observed event parameters across _all_ specified GA4 properties and event parameters defined by the [GA4 Enhanced Event Measurement](https://support.google.com/analytics/answer/9216061?hl=en&ref_topic=13367566&sjid=7618755317365101827-EU) specification for Web or App, and user properties are defined by the values observed across _all_ specified GA4 properties.

Since the same integrated decoder functions are deployed across _all_ specified GA4 properties, the output schema will be aligned across GA4 properties, enabling downstream data integration without additional transformation.

!!! info "deployment: `DEPLOY_GA4_CUSTOM_WEB_INTEGRATED`"
    === "EU"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_CUSTOM_WEB_INTEGRATED(ga4_dataset_ids, integrated_dataset_id, conversion_events, event_options);
        ```

    === "US"
        ```sql
        CALL decodedata.us.DEPLOY_GA4_CUSTOM_WEB_INTEGRATED(ga4_dataset_ids, integrated_dataset_id, conversion_events, event_options);
        ```

!!! info "deployment: `DEPLOY_GA4_CUSTOM_APP_INTEGRATED`"
    === "EU"
        ```sql
        CALL decodedata.eu.DEPLOY_GA4_CUSTOM_APP_INTEGRATED(ga4_dataset_ids, integrated_dataset_id, conversion_events, event_options);
        ```

    === "US"
        ```sql
        CALL decodedata.us.DEPLOY_GA4_CUSTOM_APP_INTEGRATED(ga4_dataset_ids, integrated_dataset_id, conversion_events, event_options);
        ```
    
    