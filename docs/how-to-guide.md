# How-to Guide

## SQL Blocks Variable
The first thing you need to do when building a transformation flow is declaring the sql_blocks variable which will hold each transformation block name, sql code and dependencies.  This is an ARRAY of STRUCTs, with each STRUCT composed of the following elements:

1. **sql_block.name** `STRING`: unique name of the block, which will be the name of the corresponding Common Table Expression
1. **sql_block.code** `STRING`: SQL code to execute the transformation.  Note that this block can reference external tables and/or sql_blocks which occur above this block.
1. **sql_block.dependencies** `ARRAY<STRING>`: references to external tables and/or sql_blocks.

THe standard code required to iniitalise a transformation flow is therefore:

```
DECLARE sql_blocks ARRAY<STRUCT<name STRING, code STRING, dependencies ARRAY<STRING>>>
```


