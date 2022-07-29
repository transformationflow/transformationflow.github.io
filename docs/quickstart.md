---
hide:
  - navigation
---
## Key Concepts
### Partitioned Tables
Partitioned tables are the foundation for efficient data transformation flows.  In partitioned tables, the data is actually physically stored in sequential partitions, meaning that if you know the partition(s) you are looking for it is quick and cost-effective to access that data.  They eliminate the need for expensive and slow full-table-scans and help avoid unnecessarily processing data on a repeating basis.

They also enable optimised downstream performance in BI tools such as Data Studio.  If you set the date parameters in a Google Data Studio report to be a date-partitioned field then the data transfer between BigQuery and Data Studio is limited just to the partitions required, increasing responsiveness and improving the end-user experience. 

### Date Sharded Tables
Date sharded tables are similar to date partitioned tables, but with some key differences.  They are actually separate tables with a common `table_name_stem_` and a date suffix in the format `YYYYMMDD`.  In the user interface they appear to be a single table with a dropdown to select the date.

This means that the individual shards can be scanned without requiring a full table scan, using `_TABLE_SUFFIX` pseudocolumn and a wildcard in the query.  You can limit the shards scanned using a number of different SQL constructs:

=== "="
    ```sql
    SELECT *
    FROM `project_id.firebase_dataset.events_*`
    WHERE _TABLE_SUFFIX = '20220701'
    ```

=== ">="
    ```sql
    SELECT *
    FROM `project_id.firebase_dataset.events_*`
    WHERE _TABLE_SUFFIX >= '20220701'
    ```

=== "BETWEEN"
    ```sql
    SELECT *
    FROM `project_id.firebase_dataset.events_*`
    WHERE _TABLE_SUFFIX BETWEEN '20220701' AND '20220731'
    ```

This is the mechanism we use to limit the data scanned every time the flow is executed, so instead of reprocessing _all_ data every day, only the incremental data is updated.  

### SQL Builder Functions
One current major limitation of BigQuery Routines (i.e. `PROCEDURES`, `USER DEFINED FUNCTIONS (UDF)` & `TABLE FUNCTIONS`) is that you cannot use a parameter to define a structural element, like a column name, table name or identifier.  In order to circumvent this limitation we leverage SQL Builder functions, which are SQL UDFs which dynamically construct SQL statements based on input parameters. This SQL can then be executed as part of a routine, and the execution outputs can then be returned and used in subsequent actions.

These functions are stored in datasets with a `_sqlb` suffix, and the function names are also prefixed with a `sqlb__`.

#### SQL Builder Function Execution
To execute a `sqlb` function you simply wrap a simple `SELECT` statement in an `EXECUTE IMMEDIATE` statement, executing the constructed SQL.  For example:
```sql
EXECUTE IMMEDIATE ((
SELECT `flowfunctionseu.metadata_sqlb.sqlb__get_column_schema`(
  'project_id.dataset_name.table_name'
  )
));
```

#### SQL Builder Function Output
The real power of these functions are, however, when you can use the outputs in subsequent operations.  In order to do this you need to know the exact structure you are expecting to be returned, and you then set the value of the variable using the `EXECUTE IMMEDIATE... INTO...` construct.  In the following example, the function `sqlb__get_column_schema` returns a `STRUCT` containing information about the table columns in a variety of forms. To pick one of them (e.g. `schema_multiline`) you simple use a `'.schema_multiline'` suffix:

```sql
DECLARE table_column_schema STRING;

EXECUTE IMMEDIATE ((
SELECT `flowfunctionseu.metadata_sqlb.sqlb__get_column_schema`(
  'project_id.dataset_name.table_name'
  )
)) INTO table_column_schema
```





## Flow Configuration
Flow configuration takes place inside the main orchestrator function for simplicity, although it could technically reside in a BigQuery table, external table (e.g. Google Sheet) or anywhere else the data can be accessed from BigQuery (e.g. JSON files in a GCS bucket or even an external database).  

There are two configurations required: `flow_input_config` and `flow_output_config`.

### Input Configuration: `flow_input_config`
The flow input configuration (`flow_input_config`) defines each source table (`source_table_ref`), source table partitioning column (`source_table_partitioning`) and the view ref (`input_view_ref`) of the entry point of the source data subset into into the transformation flow.  It is represented by an ARRAY of STRUCTS with the following schema:

=== "Schema"
    ```sql
    ARRAY<STRUCT<source_table_ref STRING, source_table_partitioning STRING, input_view_ref STRING>>
    ```

=== "Declaration"
    ```sql
    DECLARE flow_input_config 
    ARRAY<STRUCT<source_table_ref STRING, source_table_partitioning STRING, input_view_ref STRING>> 
    DEFAULT [
    ('project_id.firebase_dataset.events_*', '_TABLE_SUFFIX', 'project_id.events_flow.001_inbound_events')
    ]; 
    ```

Note that as the example is a Firebase sharded table, in the configuration declaration we set the `source_table_partitioning` to `_TABLE_SUFFIX`. For partitioned source tables we would simply use the partition column name.


### Output Configuration: `flow_output_config`
The flow output configuration (`flow_output_config`) defines the target output tables (`output_table_ref`), partitioning (`output_table_partitioning`) and the source table or view from which to feed the output table (`output_source_ref`).  It is represented by an ARRAY of STRUCTS with the following schema.

=== "Schema"
    ```sql
    ARRAY<STRUCT<output_table_ref STRING, output_table_partitioning STRING, output_source_ref STRING>>
    ```

=== "Declaration"
    ```sql
    DECLARE flow_output_config 
    ARRAY<STRUCT<output_table_ref STRING, output_table_partitioning STRING, output_source_ref STRING>>
    DEFAULT [
    ('project_id.events_flow._bi_events_with_flags', 'event_date','project_id.events_flow.402_events_with_flags'),
    ('project_id.events_flow._bi_summary_by_week', '','project_id.events_flow.601_summary_by_week')
    ];
    ```

This output configuration will feed two tables which the downstream dashboards can use as data sources, one partitioned by `event_date` and one unpartitioned summary.  Note that in this case all output tables are prefixed with a `_bi`, which is a naming convention chosen to identify tables to which downstream Business Intelligence tools should connect.

Also note that to leave `output_table_partitioning` underfined, you need to pass an empty string (`''`) as the parameter value.

### Flow Orchestrator Function: `run_transformation_flow`
In order to keep the management and operation as simple and consistent as possible, there is only one function used to run the flow, which takes a single integer parameter depending on the number of days in addition to today which are to be processed:

=== "Syntax"
    ```sql
    CALL `project_id.firebase_dataset.run_transformation_flow`(last_n_days_to_replace INT64)
    ```

=== "Invocation (3 days data)"
    ```sql
    CALL `project_id.firebase_dataset.run_transformation_flow`(3)
    ```

### Flow Orchestrator Logic
The flow orchestrator function `run_transformation_flow` executes the flow according the the following logical sequence:

#### Top-Level Logic 
[![Top-Level Logic](https://mermaid.ink/img/pako:eNqVU11L7DAQ_SshsERxFT_eysUXrU8-iKsimEuYbbK7gXZS8qF3b-l_N0lX2UIX9C0958zMOR2mo5WRihZ0Nus0al-Qjq1q81FtwHpWdKwK9l2xgrAlOO3YnDCpYW2heQApNa4jdXneR9hvVJOFqIK3ULMv7AWshmWtHMvNDfo7aHS9TdrGoHEtVCqpE7XQ_1OTi_O-72czjhy_3ZD7x_TtwjKObzckEUL9U1Xw2uAbpxdnpHwtb56fysxx-pejDSiiG3QrYxtIQpG4oyg_QP1Z2uujGpwXKCRsnfBGWNXW0eRxbHnMUaEcGTHBt8GLyuBKr2PjqzOyKJ-eH8jJyI8YdNnWWnmxh-1qk6tpZhgcWglefTE-_9RUM4X_PobGUYrLAymybBxiv3KUYZ8YhjrlQ7vDo2kQ71p95BDTzI9jHNgmOT29JtN-pv3ngmkvPx4zXt2BZeeSqc3ROW1UbK5lPMuOIyGc5kPitIjPeIfxxbGPuqG8lNobS4sV1E7NKQRvFlusvoFBdTuc7Q7tPwFUIXjH)](https://mermaid-js.github.io/mermaid-live-editor/edit#pako:eNqVU11L7DAQ_SshsERxFT_eysUXrU8-iKsimEuYbbK7gXZS8qF3b-l_N0lX2UIX9C0958zMOR2mo5WRihZ0Nus0al-Qjq1q81FtwHpWdKwK9l2xgrAlOO3YnDCpYW2heQApNa4jdXneR9hvVJOFqIK3ULMv7AWshmWtHMvNDfo7aHS9TdrGoHEtVCqpE7XQ_1OTi_O-72czjhy_3ZD7x_TtwjKObzckEUL9U1Xw2uAbpxdnpHwtb56fysxx-pejDSiiG3QrYxtIQpG4oyg_QP1Z2uujGpwXKCRsnfBGWNXW0eRxbHnMUaEcGTHBt8GLyuBKr2PjqzOyKJ-eH8jJyI8YdNnWWnmxh-1qk6tpZhgcWglefTE-_9RUM4X_PobGUYrLAymybBxiv3KUYZ8YhjrlQ7vDo2kQ71p95BDTzI9jHNgmOT29JtN-pv3ngmkvPx4zXt2BZeeSqc3ROW1UbK5lPMuOIyGc5kPitIjPeIfxxbGPuqG8lNobS4sV1E7NKQRvFlusvoFBdTuc7Q7tPwFUIXjH)

#### Flow Input Logic
[![Flow Input Logic](https://mermaid.ink/img/pako:eNqdU9FqGzEQ_BUhMHLAgbSPRwk0rUPzkpS6FEpUxFpa26I66ZB0Ce7l_r2rs-u4rd0kfTmk2dnZ3dFtx3UwyCs-GnXW21yxTixcuNcriFlUndBtvENRMTGHZJOYMGEsLCPUH8EY65cUen3WE5xXWA9Ej22O4MQv7AtEC3OHSQziwedLqK1bF24dfEgNaCzsEprZH0Xk1Vnf96OR9NLvumGfL8o9tXMq36yY9U2blQ5-YZe3kjPJv1EUc9uoTchABnVn8T6NKX448mYez8cOUlae0HVSOaiIjaOWTkjwZK_eYQFlMIN1VODy5hObvn33gaXQRo0ql6HZ1TUrE6j9bodO9Qr19y3c0Hg22-DJ0E7yq8TIudLavlS574hoJO-Hcf_sqEywn_YoriL4JW69eGnaM4w6LLponftNeXz7XGZRRW-2n2OPy05Pz9kxN4_7PKQ9SP51OpP8gf2HJU9rX9_8Q_qvcaXnE15jrMEa2shOekY_9bBDkld0pBWkk_Q98dqGxHBqbA6RVwtwCScc2hxma693wIb1frOxW7T_CWfUfVU)](https://mermaid-js.github.io/mermaid-live-editor/edit#pako:eNqdU9FqGzEQ_BUhMHLAgbSPRwk0rUPzkpS6FEpUxFpa26I66ZB0Ce7l_r2rs-u4rd0kfTmk2dnZ3dFtx3UwyCs-GnXW21yxTixcuNcriFlUndBtvENRMTGHZJOYMGEsLCPUH8EY65cUen3WE5xXWA9Ej22O4MQv7AtEC3OHSQziwedLqK1bF24dfEgNaCzsEprZH0Xk1Vnf96OR9NLvumGfL8o9tXMq36yY9U2blQ5-YZe3kjPJv1EUc9uoTchABnVn8T6NKX448mYez8cOUlae0HVSOaiIjaOWTkjwZK_eYQFlMIN1VODy5hObvn33gaXQRo0ql6HZ1TUrE6j9bodO9Qr19y3c0Hg22-DJ0E7yq8TIudLavlS574hoJO-Hcf_sqEywn_YoriL4JW69eGnaM4w6LLponftNeXz7XGZRRW-2n2OPy05Pz9kxN4_7PKQ9SP51OpP8gf2HJU9rX9_8Q_qvcaXnE15jrMEa2shOekY_9bBDkld0pBWkk_Q98dqGxHBqbA6RVwtwCScc2hxma693wIb1frOxW7T_CWfUfVU)

#### Flow Output Logic
[![FLow Output Logic](https://mermaid.ink/img/pako:eNqVU11LIzEU_SshUFKhgu7jIIKrFX1RscuCmCXcTtJO2Ewy5EPpjvPf92Zaa4XR6suQnHtycu7J3JaWTipa0NGo1VbHgrRsYdxzWYGPrGhZmfyTYgVhcwg6sAlhUsPSQ30HUmq7xNKPow7hWKm6J1qVogfDXrHf4DXMjQqsF3c2XkKtzSpza2ddaKBUmZ1LM_0vixwfdV03GnHL7dYN-fUz70Oa4_VNRVyKTYqidHahl4-cEk7_cJsaCVGJTTH2F4-xOoSfzP3p2ECIwgoJqyCiE141Bv0coNjBzmVDx4VUEbRB8cvbezI9O78iu3VyfUOyefHOaG-yrFT59xVvsDcdtbOYZsvpdSAYW7a2K5b3W6KSnHbcbqy-M_Wm1ne9h7I_gEGBZx0rsUjGiOCSx2oPjx-_Qc7aysrNZ_jZyOHhKfkwqk9S7A--cPownXH6QvaE8AWhm9sPdYbboxNaK1-DljhaLbcEf85-GDgtcImzhCtuO-StO59KHZ2nxQJMUBMKKbrZypZbYM26WI_eBu3-A4_WZ6A)](https://mermaid-js.github.io/mermaid-live-editor/edit#pako:eNqVU11LIzEU_SshUFKhgu7jIIKrFX1RscuCmCXcTtJO2Ewy5EPpjvPf92Zaa4XR6suQnHtycu7J3JaWTipa0NGo1VbHgrRsYdxzWYGPrGhZmfyTYgVhcwg6sAlhUsPSQ30HUmq7xNKPow7hWKm6J1qVogfDXrHf4DXMjQqsF3c2XkKtzSpza2ddaKBUmZ1LM_0vixwfdV03GnHL7dYN-fUz70Oa4_VNRVyKTYqidHahl4-cEk7_cJsaCVGJTTH2F4-xOoSfzP3p2ECIwgoJqyCiE141Bv0coNjBzmVDx4VUEbRB8cvbezI9O78iu3VyfUOyefHOaG-yrFT59xVvsDcdtbOYZsvpdSAYW7a2K5b3W6KSnHbcbqy-M_Wm1ne9h7I_gEGBZx0rsUjGiOCSx2oPjx-_Qc7aysrNZ_jZyOHhKfkwqk9S7A--cPownXH6QvaE8AWhm9sPdYbboxNaK1-DljhaLbcEf85-GDgtcImzhCtuO-StO59KHZ2nxQJMUBMKKbrZypZbYM26WI_eBu3-A4_WZ6A)





