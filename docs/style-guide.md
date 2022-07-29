---
hide:
  - navigation
---

# Transformation Flow Style Guide
These guidelines are consistently applied when we build data transformation flows, and serve a few purposes:

- **Consistency** across transformation flows
- **Clarity** on how to think about and work with data
- **Readability** of code and logical flow
- **Capability** extensions from the `flowfunctions` library

## Naming Conventions
A Transformation Flow is the end-to-end flow between inbound data and transformed outbound data.  It is a set of inbound tables, followed by some intermediate transformation logic and a set of outbound tables.  The important definitions here are:

- **Flow Dataset:** A dataset containing individual stages (connected tables and views)
- **Flow Stage:** A single table or view in a transformation flow   
- **Flow Step:** A common table expression within a SQL transformation flow stage

=== "Flow Datasets"
    A transformation flow name is the shortest description of the flow, which will be used as the base for all related datasets. It might be as short as a single word (e.g. `shopify`) or multiple words sepearated by underscores (e.g. `google_analytics`).
    
    Suffixes are used to group datasets within transformation flows, added to the transformation flow name.  This means that the flow datasets will appear adjacent to each other in the user interface or in any ordered list. The following suffixes are recommended at a minimum:

    - **`_admin`:** for administrative tables, views and custom functions (e.g. inbound data monitoring, GCS bucket inventory or table refresh automation)
    - **`_inbound`** or no suffix**:** for inbound tables only
    - **`_transform`** or **`_flow`**: core transformation views, intermediate tables where required and outbound views/tables 

    These datasets might also be required when undertaking ad-hoc data quality checks or performing staged transformation flow development:

    - **`_qa`:** ad-hoc data quality checks and QA development.
    - **`_staging`:** a parallel transformation flow replica for development without impacting an existing live transformation flow

=== "Flow Stages"
    Flow stage names consist of two distinct and important parts, separated by a single underscore:

    - **Flow Stage Index (`flow_stage_index`):** a three-to four digit numerical identifier[^1] (e.g. `201`)
    - **Flow Stage Title (`flow_stage_title`):** a descriptive title of the outcome of the flow stage (e.g. `deduplicate_sessions`)

    The `flow_stage_index` and `flow_stage_title` are then combined into the `flow_stage_name` (e.g. `201_deduplicate_sessions`).  This means that each flow stage name is descriptive enough to be human readable, and also is sequenced logically when reading the stages alphabetically.

    The only precise rules for definition of the indices and titles are:

    - The `flow_stage_index` and `flow_stage_title` should be separated by a single underscore (`_`) as the `flow_stage_index` will be parsed from the name
    - Flow Stages can only reference other Flow Stages with lower indices (e.g. `201` can reference `107` but not `305`)
    - Flow Stage titles should begin with an active verb where possible, and should be a concise description of the objective/outcome (e.g. `aggregate_to_sessions`, `deduplicate_users`)

    Note that if a flow stage is an exact replica of an upstream flow stage, then the `flow_stage_title` needs to be identical, with an incremented `flow_stage_index`.  For example if you want to include a match table from Google Sheets but do not want downstream data consumers to require federated access to the source sheet:
    
    - `010_match_table_usergroups`
    - `011_match_table_usergroups`

    The identical `flow_stage_title` means that these will be parsed as related when mapping dependencies, and the sequence of the index means that `011` will be after (and therefore dependent on) `010` in all dependency representations.

=== "Flow Steps"
    Flow steps are the common table expressions which combine into individual flow stages.  This style is very important as it supports building the transformation flow stages in a clear, extensibe, testable and parseable manner.  This code structure is:

    ```sql
    WITH
    inbound_data AS (
    SELECT *
    FROM project.dataset.table
    ),

    do_something AS (
    SELECT
    [TRANSFORM_SQL_QUERY]
    FROM inbound_data
    )

    SELECT *
    FROM do_something
    ```
    
    These should be named using the following conventions:

    - Begin with an active verb followed by a more detailed description of the stage objective/outcome (e.g. `unnest_hits_array`, `flag_duplicate_rows`, `join_user_groupings_to_user_data`).  Conciseness is not so much of an issue here as long names will not be truncated in the user interface.
    - Make clear, specific and unambiguous references to specific actions and/or columns.  
    - Do not give a flow step the same name as an existing column or anything else which might make the code confusing and/or ambiguous. 

---

## Data Ingestion
Moving data around consistently, reliably and at scale is a complex problem to solve. However there are many potential solutions, especially when working with BigQuery.  The objective here is to land your data consistently into BigQuery in a low-cost (or ideally zero-cost), reliable and low maintenance manner.

=== "Native Services"
    Using native services should be the preference here, whether it is direct integration with a Google product,
    direct connection from an external supplier or by leveraging a Data Transfer into BigQuery or Google Cloud Storage.  

=== "External Tables"
    External tables can be a very low-maintenance solution to data ingestion requirements, as data can be directly queried from Google Cloud Storage or Amazon S3 buckets without requiring loading of the data.  Files with the schema included (i.e. Parquet or Avro) are preferable to CSV or JSON files.

=== "3rd Party Services" 
    3rd Party Services are essential when you need to integrate data sources without native connectors. We recommend that you pick a single service, and one where your costs do not scale with the volume of data ingested.  Currently we recommend:

    - [Airbyte](https://airbyte.com/): If you are comfortable deploying and maintaining your own instance on a virtual machine.
    - [Dataddo](https://www.dataddo.com/): If you are not.

    Both are extensible, however Dataddo will build new connectors for you at no cost.  Airbyte will require more technical input (predominantly Python/Java), but has a friendly, active and fast-growing community.

=== "Custom Development" 
    This is almost always a bad idea, even if you do have the technical capabilities and resources.  Continue down this path at your peril... then use a 3rd party specialist or platform to do this complex job reliably.

---

## Table Types
The different table types in BigQuery are the building blocks of any transformation flow, and understanding the differences is critical to building robust and efficient flows.

=== "Tables"
    Tables are the base-level building blocks of BigQuery, however they are the least used in transformation flows.  Tables have a specific schema and you load data directly into them, however partitioned tables are preferred in most situations. 

=== "External Tables (Storage)" 
    External tables enable direct querying of data files in Google Cloud Storage (GCS) buckets, as well as AWS S3 or Azure Storage buckets.  There are some complexitites and limitations based on e.g. folder structure and file naming, however they can be used to build powerful, robust data ingestion points for your transformation flows. 
    
    They also add redundancy and resilience to your data flows as deleting the external table does not delete the underlying data. 

=== "External Tables (Google Sheets)" 
    External tables can also be created from sheets in a Google Sheets workbook, which is a powerful mechanism but not without potential pitfalls.  They are very useful as e.g. match tables or for semi-automated QA based on point-in-time data extraction from another non-integrated system.  However some potential challenges are:

    1. Schema Detection does not work when all columns are detected as a string type.  
    2. Downstream users and systems will require federated access to the sheets.

    Point 1 can be addressed by adding an incremental integer `row_id` into the sheet and point 2 by creating a related flow stage (with the same `flow_stage_title` but an incremented `flow_stage_index`) as a table.  

    They can also be used as temporary data ingestion points (if an external system can output to Google Sheets), however this is not recommended as a long-term approach as you can quickly hit row limits and performance challenges.

=== "Views" 
    Views are the foundational building block of transformation flows and are the mechanism by which all transformation/integration logic is written in pure SQL, as well as the functions in our library `flowfunctions`. 

=== "Partitioned Tables" 
    Partitioned tables are another critical building block which should be leveraged in all but the simplest data transformation flows.  Essentially they allow physical segregation of data based on a date or timstamp field (or ingestion date/time), which has a number of positive impacts:

    - **Performance Improvement:** Dashboard performance is greatly improved due to efficient querying.
    - **QA Efficiency:** Spot checks can be done on specfic partitions without full table scans.
    - **Ongoing Cost Optimisation:** Data volume is minimised, also reducing cost.

    It is also possible to further optimise cost by applying transformation flows to a dynamic subset of partitions (e.g. applying full transformation logic to the inbound partition from yesterday and appending to output partitioned table).    

---

## Column Naming Convention
=== "Case" 
    Columns are always in Camel Case (i.e. lower case, separated by underscores e.g. `video_views` or `media_id`).  If the column type is an array, they suffix `_array` is added to the column name.

=== "Aggregate Columns" 
    When a column is aggregated, in order to preserve the relationship in the flow profile the new name should be descriptive, but include the source name as a substring (e.g. `sum_video_views` or `total_video_views`).  This is so the columns can be related across flow stages using simple string comparisons. 

=== "Related Columns" 
    Similar to aggregate columns, to maintain the relationship when profiling across the entire flow the derived column name should contain the source column name (e.g. `media_id_deleted_flag` or `valid_video_views_flag`).

---

## Deployment
=== "Inbound Partitioned Tables" 
    Partitioned tables are preferred for inbound data, as when performing QA operations it grreatly improves query speed and reduces cost.  If 3rd party suppliers do not directly support this, it is worth testing if they can load data into a partitioned table as this is often the case.

=== "Intermediate Tables" 
    For complex flows (or flows where intermediate stages are referenced by a number of downstream stages) it can be beneficial for development, debugging and performance to build intermediate aggregated tables, which will be refreshed using a custom function in the `_admin` dataset.

=== "Outbound Partitioned Tables" 
    Outbound tables will almost always be partitioned, either on the ingestion date or a specific date field.  This results in cost and performance benefits, although there are some complexities to be addressed in order to leverage  full optimisation from partition subset operations.

=== "Partition Subset Operations" 
    One of the major benfits of partitioned tables the fact that you can operate on a specific subset of partitions and therefore limit unnecessary reprocessing of data.  There are some deployment complexities as to leverage this effiency you technically need to hard-code the partition(s) you want to reference, however you can deploy a flow using this pattern using some of our core `flowfunctions`.

=== "Scheduled Scripts" 
    Automation activities leverage scripting in BigQuery, packaging commands up into executable functions.  These functions can then be run ad-hoc in the console e.g. `CALL project.dataset_admin.refresh_tables()`or by scheduling this function call using BigQuery Scheduled Functions.  Scheduled functions should not be used for complex logic or SQL as all SQL should be in BigQuery for governance purposes.

---

## SQL Style
When you spend time writing SQL which writes other SQL (sometimes executed by other SQL), it's natural to think quite carefully about how to write SQL.  As such we have some simple guidelines which support code readability, clarity and maintainability.  Of course rules are made to be broken so these are not always set in stone, however they hold in the vast majority of use-cases.

=== "Optimise for Readbility" 
    If in doubt, decide how to structure your code based on how easily it can be understood when reading from top to bottom without any context. This is especially pertinent in e.g. CASE statements where different approaches are more/less readable depending on the situation.

=== "Use Common Table Expressions" 
    This supports modular, sequential operations, which are easier to understand, debug and extend.  Clear, consistent naming of CTEs (a.k.a. flow steps) also negates the need for comments in your code, for which there is no standardised approach in SQL.

=== "Avoid Sub-Queries" 
    Good code can be read from top to bottom, whereas nested queries need to be read from the inside out, whilst also keeping track of nesting depth _and_ potentially impactful clauses to the top _and_ the bottom of the inner clause.  Sometimes these are unavoidable (e.g. in succinct variable declaration where ordering is required) but try to use sparingly, if at all.

=== "Capitalise Keywords" 
    This is a style point, however for consistent readability we use `CAPITAL LETTERS` for all keywords and `lower_case` for all other names.  This helps make a clear distinction between different parts of a query and makes it quicker to understand the query at a glance.

=== "Left-Justify Keywords" 
    By left-justifying the keywords where possible, it becomes quicker to scan down and understand the purpose of a query without necessiating time consuming scanning across the screen.

=== "Avoid Indentation" 
    There is no consistly defined or enforced indentation system in SQL, so avoid this to avoid confusion. 

=== "Group on Specific Columns" 
    Although it is possible to use e.g. `GROUP BY 1, 2, 3`to group based on ordinal position, this should be avoided as it negatively impacts readability: the reader will need to then scan up the page to identify the precise grouping columns in the query.  It is also less robust than grouping by specific columns as re-ordering the columns might produce inconsistent and erroneious results.     


[^1]: This approach is loosely based on the [Johnny.Decimal](https://johnnydecimal.com/) file organisation system, and is intended to be simple and flexible without being overly prescriptive.




