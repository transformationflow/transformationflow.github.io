The **Decode Data** (`decodedata`) for GA4 library simplifies and augments the GA4 BigQuery export, making subsequent data operations simpler and quicker. Go to [decodedata.io](https://decodedata.io/) for detailed documentation and registration.

# Summary
_Attribute_ | Value
--- | ---
_**Name**_ | Decode Data
_**Project ID**_ | `decodedata`
_**Version**_ | `decodedata:v1.0.0`
_**Access**_ | Licensed
_**Description**_ | Enables automatic pre-modelling of the GA4 BigQuery `events_YYYYMMDD` export, providing flattened, date-partitioned `events` and `sessions` tables containing all standard and observed `event_params` and `user_properties`. It also provides mechanisms to detect new `event_params` and `user_properties` values and to incrementally update the schema based on inbound data detection. 

# Deployment
Installation and execution functions are currently deployed into the following [BigQuery regions](https://cloud.google.com/bigquery/docs/locations), but can be mirrored to additional regions as required:

Region Name | Dataset ID 
--- | --- 
EU | `bqtools.eu` 
US | `bqtools.us` 