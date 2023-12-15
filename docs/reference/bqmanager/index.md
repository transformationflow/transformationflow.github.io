The **BigQuery Manager** (`bqmanager`) library deploys a set of metadata-powered extensions to BigQuery to enable visibility and actions across multiple projects which are not possible using native BigQuery functionality.

# Summary
_Attribute_ | Value
--- | ---
_**Name**_ | BigQuery Manager
_**Project ID**_ | `bqmanager`
_**Access**_ | Licensed
_**Description**_ | BigQuery functional extensions to improve visibility and management across multiple projects.

# Deployment
Functions are currently deployed into the following [BigQuery regions](https://cloud.google.com/bigquery/docs/locations), but can be mirrored to additional regions as required:

Region Name | Dataset ID 
--- | --- 
EU | `bqmanager.eu` 
US | `bqmanager.us` 

# Pre-requisites
All functions, tables and views are deployed to the `BQMANAGER` dataset in the selected project.  This dataset needs to be created in a region which aligns to a deployed `bqmanager` dataset.