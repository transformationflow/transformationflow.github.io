Although the [INFORMATION_SCHEMA](https://cloud.google.com/bigquery/docs/information-schema-intro) provides us with some useful mechanisms for querying resource metadata, the following functions address more complex use-cases which go beyond the basic foundations.

# Row Counts
Computation of the row counts in tables and views is a simple high-level metadata analysis approach which enables potential issues to be located, and which can be extremely helpful in data QA.

## All Dataset Tables and Views
The metadata query `SELECT * FROM project_id.dataset_name.__TABLES__` returns row counts for `BASE TABLES`, but it returns row counts of zero for `VIEWS` or `EXTERNAL TABLES`.  This function iterates around all of the tables and views one by one and computes the row count of each.  When it has finished computing, the result can be viewd by clicking on the final `VIEW RESULTS` action in the results pane.

=== "us" 
    ```sql
    CALL flowfunctions.profiling.get_dataset_row_counts (
            dataset_ref -- STRING
            );
    ```

=== "eu" 
    ```sql
    CALL flowfunctionseu.profiling.get_dataset_row_counts (
            dataset_ref -- STRING
            );
    ```

If you want to use these row counts as part of a subsequent calulation or action, you have to use the function variation with an `_out` suffix, as well as declare the variable which will hold the function response:

=== "us" 
    ```sql
    DECLARE row_counts ARRAY<STRUCT<table_ref STRING, records INT64>>;
    
    CALL flowfunctions.profiling.get_dataset_row_counts (
            dataset_ref, -- STRING
            row_counts -- ARRAY<STRUCT<table_ref STRING, records INT64>>
            );
    ```

=== "eu" 
    ```sql
    DECLARE row_counts ARRAY<STRUCT<table_ref STRING, records INT64>>;

    CALL flowfunctionseu.profiling.get_dataset_row_counts (
            dataset_ref, -- STRING
            row_counts -- ARRAY<STRUCT<table_ref STRING, records INT64>>
            );
    ```

After function execution, the `row_counts` variable will be populated with the response data and can be `UNNESTED` from the array structure with the query syntax `SELECT * FROM UNNEST(row_counts)`.