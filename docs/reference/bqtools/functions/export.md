These functions are used to automate data export from BigQuery.
# **EXPORT data**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `export_data`
_**ID**_ | `bqtools.[region].export_data`
_**Version**_ | `bqtools:v1.0.0`
_**Description**_ | Exports data to Google Cloud Storage or Amazon S3 bucket, with export options aligned to the [EXPORT DATA](https://cloud.google.com/bigquery/docs/reference/standard-sql/other-statements#export_data_statement) statement.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `export_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].export_data`

!!! info "EXECUTION" 
    === "EU"
        ```sql
        DECLARE export_options JSON;

        SET export_options = JSON '{...}';

        CALL bqtools.eu.export_data(export_options);
        ```
    
    === "US"
        ```sql
        DECLARE export_options JSON;

        SET export_options = JSON '{...}';

        CALL bqtools.us.export_data(export_options)
        ```

!!! info "ARGUMENTS" 
    ARGUMENT | DATA TYPE | DESCRIPTION
    --- | --- | ---
    export_options | JSON | Options to configure specific exports, aligned with to the [EXPORT DATA](https://cloud.google.com/bigquery/docs/reference/standard-sql/other-statements#export_data_statement) statement.

??? note "EXPORT OPTIONS"
    === "export_options"
        Option | Data Type | Values | JSON Path | Default | Description
        --- | --- | --- | --- | --- | ---
        query_statement | STRING | SQL Query | `query_statement` | `REQUIRED` | An SQL query. The query result is exported to the external destination.
        format | STRING | AVRO, CSV, JSON, PARQUET | `format` | `REQUIRED` | The format of the exported data.
        uri | STRING | Single-wildcard GCS URI | `uri` | `REQUIRED` | The destination URI for the export.
        connection_name | STRING | `PROJECT_ID.LOCATION.CONNECTION_ID` | `connection_name` | `None` | Specifies a connection that has credentials for accessing the external data.
        compression | STRING | GZIP, DEFLATE, SNAPPY | `compression` | `None` | Specifies a compression format. If not specified, the exported files are uncompressed.
        overwrite | BOOL | `true`/`false` | `overwrite` | `false` | If true, overwrites any existing files with the same URI. Otherwise, if the destination storage bucket is not empty, the statement returns an error.
        header | BOOL | `true`/`false` | `header` | `false` | If true, generates column headers for the first row of each data file. Applies to: CSV.
        field_delimiter | STRING | Delimiter string | `field_delimiter` | `,` (comma) | The delimiter used to separate fields.
        use_avro_logical_types | BOOL | `true`/`false` | `use_avro_logical_types` | `false` | Whether to use appropriate AVRO logical types when exporting TIMESTAMP, DATETIME, TIME and DATE types. Applies to: AVRO

    === "Examples"
        Examples are aligned to the BigQuery [EXPORT DATA](https://cloud.google.com/bigquery/docs/reference/standard-sql/other-statements#export_data_statement) statement examples.
        
        === "Export data to Cloud Storage in CSV format"

            === "Original"
            
                ```sql
                EXPORT DATA 
                OPTIONS (
                    uri='gs://bucket/folder/*.csv',
                    format='CSV',
                    overwrite=true,
                    header=true,
                    field_delimiter=';') 
                AS
                SELECT field1, field2 
                FROM mydataset.table1 
                ORDER BY field1 LIMIT 10
                ```

            === "Functional"
            
                ```sql
                DECLARE export_options JSON;
                SET export_options = JSON """{
                    "uri": "gs://bucket/folder/*.csv",
                    "format": "CSV",
                    "overwrite": true,
                    "header": true,
                    "field_delimiter": ";",
                    "query_statement": "SELECT field1, field2 FROM mydataset.table1 ORDER BY field1 LIMIT 10"
                    }""";

                CALL bqtools.us.export_data(export_options)
                ```