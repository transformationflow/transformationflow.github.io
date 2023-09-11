The Decode Data for GA4 (`decodedata-ga4`) library enables Google Analytics 4 users to automatically remodel their raw event data in BigQuery, making the data simpler to use and more efficient to query. 

# Summary
_Attribute_ | Value
--- | ---
_**Name**_ | Decode Data for GA4
_**Project ID**_ | `decodedata-ga4`
_**Access**_ | Licensed
_**Description**_ | Analyses event data, then builds and deploys a property-specific date-parameterised table function to restructure the data into a simple flat structure.  It adds count metrics for `event_names` and safe/hungry type-specific value columns for `event_params` and `user_properties`.

# Deployment
Functions are currently deployed into the following [BigQuery regions](https://cloud.google.com/bigquery/docs/locations), but can be mirrored to additional regions as required:

Region Name | Dataset ID 
--- | --- 
EU | `bqtools.eu` 
US | `bqtools.us` 