The **Decode Data** (`decodedata`) library enables dynamic remodelling of the Google Analytics 4 to BigQuery data export, flattening the data without information loss, making it simpler to use and more efficient to query.

# Summary
_Attribute_ | Value
--- | ---
_**Name**_ | Decode Data
_**Project ID**_ | `decodedata`
_**Access**_ | Licensed
_**Description**_ | Profiles historic event data, then builds and deploys a base query and set of bespoke functions to remodel the data into a simple flat structure.  It adds count metrics for `event_names` and type-specific value columns for `event_params` and `user_properties`.

# Deployment
Functions are currently deployed into the following [BigQuery regions](https://cloud.google.com/bigquery/docs/locations), but can be mirrored to additional regions as required:

Region Name | Dataset ID 
--- | --- 
EU | `bqtools.eu` 
US | `bqtools.us` 