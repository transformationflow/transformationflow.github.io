---
hide:
  - navigation
---
# Introduction
This quickstart will introduce you to the small set of `flowfunctions` which will enable you to rapidly build high quality, tested data transformation flows. 

## Build View Outline `(flowfunctions.build.view)`
This is the first function you need when commencing the build of *any* data transformation flow or flow stage.

### Objective
Build a base outline for a transformation flow stage as a single view, including profiling and helper functions.

### Syntax
To create a new flow stage outline, call the `flowfunctions.build.view` function from your query editor, using the following syntax and arguments:

```sql 
CALL flowfunctions.build.view(
destination_view_ref,
source_ref_array,
view_step_array
)
```


### Arguments
These argument names are consistent throughout the flowfunctions library, so it is worthwhile to review them:

Argument | Data Type | Description | Example
--- | --- | --- | ---
destination_view_ref | STRING | The reference of the view to be created | `'project_id.dataset_name.destination_X'`
source_ref_array | ARRAY<STRING\> | An array of view/table references of source data | `['project_id.dataset_name.source_A', 'project_id.dataset_name.source_B']`
view_step_array | ARRAY<STRING\> | An array of names for the transformation steps | `['union_source_A_and_source_B', 'change_data_types', 'fill_null_metrics_with_zeroes', 'drop_columns']`

The `view_step_array` determines the names of the steps within the transformation view.  The naming convention helps with understanding the purpose of each stage, as reading the steps from top to bottom should enable anybody reading the code to quickly understand the code objectives and inspect the code without requiring any comments in the SQL.  Each step should be begin with an active verb which describes the action, and - if potentially ambiguous - the input data for the view step.

### Output
A view will be created with functional SQL containing sequential Common Table Expressions (steps) as well as several helper and profiler steps.  The final select clause will always be:

```sql 
SELECT * FROM flow
```

This will evaluate the current result of the transformation flow stage.  Additional helper functions are in development, however an example of a core function to accelerate your workflow is that by replacing the final select statement with:

```sql 
SELECT * FROM profile
```

The results window will display the shape of all inbound data sets, along with the shape of the current saved view.  This enables rapid checking of row counts, column count, column types and ordinal positions without leaving the console or the transformation (and your) flow.