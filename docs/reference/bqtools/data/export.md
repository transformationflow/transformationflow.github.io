These functions are used to export data from BigQuery.

# Functions
## **`export_data`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `export_data`
_**ID**_ | `bqtools.[region].export_data`
_**Description**_ | Exports the data selected by a `query_statement` into a Google Cloud Storage or Amazon S3 Bucket. Export options are aligned to the [EXPORT DATA](https://cloud.google.com/bigquery/docs/reference/standard-sql/other-statements#export_data_statement) statement.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `query_statement STRING, export_format STRING, export_uri STRING, export_option_list JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].export_data`

!!! info "execution: `export_data`"
    === "EU"
        ```sql
        SELECT bqtools.eu.export_data(query_statement, export_format, export_uri, export_option_list);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.export_data(query_statement, export_format, export_uri, export_option_list);
        ```