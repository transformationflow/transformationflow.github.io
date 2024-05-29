Resources can be types of tables, views or routines, referenced by the `resource_id = project_id.dataset_name.resource_name`.

# Date-Bounded Table Function
A table function is identical to a view, except it can accept arguments.  This is especially useful when working with date-partitioned or date-sharded source data, as by using `start_date` and `end_date` as table function `DATE` arguments, only the specific date range requested is queried or billed for. A Date-Bounded Table Function (DBTF) has precisely two `DATE` parameters as arguments: `start_date` and `end_date`.

The syntax for creating a Date-Bounded Table Function is:

```sql
CREATE OR REPLACE TABLE FUNCTION `project_id.dataset_id.transform_function_name` (start_date DATE, end_date DATE)
AS
WITH get_source_data AS (
    SELECT * 
    FROM `project_id.dataset_id.table_name`
    WHERE date_column BETWEEN start_date AND end_date)...

    [...]

SELECT *
FROM ...
```

