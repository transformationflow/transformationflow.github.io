These functions are used to create resources from arbitrary SQL definitions and options.

# Functions
## **`create_table`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `create_table`
_**Description**_ | Creates or replaces a single `BASE TABLE` defined by an arbitrary sql `query_statement`, with options defined in alignment with the `CREATE TABLE` [DDL statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_table_statement).
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `table_id STRING, query_statement STRING, table_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_table`

!!! warning "Beta"
    This `create_table` function is in beta as not all of the `table_options` have been implemented.

!!! info "execution: `create_table`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_table(table_id, query_statement, table_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_table(table_id, query_statement, table_options);
        ```

## **`create_view`**
_**Attribute**_ | Value
--- | ---
_**Function Name**_ | `create_view`
_**Description**_ | Creates or replaces a single `VIEW` defined by an arbitrary sql `query_statement`, with options defined in alignment with the `CREATE VIEW` [DDL statement](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language#create_view_statement).
_**Function Type**_ | `PROCEDURE`
_**Arguments**_ | `view_id STRING, query_statement STRING, view_options JSON`
_**Returns**_ | `None`
_**Dependencies**_ | `bqtools-qb.[region].create_view`

!!! warning "Beta"
    This `create_view` function is in beta as not all of the `view_options` have been implemented.

!!! info "execution: `create_view`"
    === "EU"
        ```sql
        CALL bqtools.eu.create_view(view_id, query_statement, view_options);
        ```

    === "US"
        ```sql
        CALL bqtools.us.create_view(view_id, query_statement, view_options);
        ```