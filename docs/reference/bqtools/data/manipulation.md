These functions are used to manipulate data in existing tables.

# **`insert_data`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `insert_data`
_**ID**_ | `bqtools.[region].insert_data`
_**Description**_ | Inserts data into an existing table using the `INSERT` [DML Statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/dml-syntax#insert_statement). The schema of the data returned by executing the `insert_sql` `STRING` must exactly match the destination table.
_**Type**_ | `PROCEDURE`
_**Arguments**_ | `destination_table_id STRING, insert_sql STRING`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].insert_data`

!!! info "execution: `insert`"
    === "EU"
        ```sql
        CALL bqtools.eu.insert_data(destination_table_id, insert_sql);
        ```

    === "US"
        ```sql
        CALL bqtools.us.insert_data(destination_table_id, insert_sql);
        ```
