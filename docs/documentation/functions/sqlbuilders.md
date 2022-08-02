One current limitation of BigQuery Routines (i.e. `PROCEDURES`, `USER DEFINED FUNCTIONS` & `TABLE FUNCTIONS`) is that you cannot use a parameter to define a structural element, like a column name, table name or identifier. In order to circumvent this limitation we leverage SQL Builder functions, which are SQL UDFs which dynamically construct SQL statements based on input parameters. This SQL can then be executed as part of a routine, and the execution outputs can then be returned and used in subsequent actions.

These functions are stored in datasets with a `_sqlb` suffix, and the function names are also prefixed with a `sqlb__`.

# SQL Builder Function Execution
To execute a sqlb function you simply wrap a simple `SELECT` statement in an `EXECUTE IMMEDIATE` statement, executing the constructed SQL. For example:

```sql
EXECUTE IMMEDIATE ((
SELECT `flowfunctions.dataset_sqlb.sqlb__function_name`(
  'project_id.dataset_name.table_name'
  )
));
```

# SQL Builder Function Output
The real power of these functions are, however, when you can use the outputs in subsequent operations. In order to do this you need to know the exact structure you are expecting to be returned, and you then set the value of the variable using the `EXECUTE IMMEDIATE... INTO...` construct. In this example the executed function returns a `STRING`.

```sql
DECLARE function_output STRING;

EXECUTE IMMEDIATE ((
SELECT `flowfunctions.dataset_sqlb.sqlb__function_name`(
  'project_id.dataset_name.table_name'
  )
)) INTO function_output;
```