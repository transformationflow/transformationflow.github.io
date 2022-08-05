These guidelines are consistently applied when we build data transformation flows, and serve a few purposes:

- **Consistency** across transformation flows
- **Clarity** on how to think about and work with data
- **Readability** of code and logical flow
- **Capability** extensions from the `flowfunctions` library

## SQL Style
When you spend time writing SQL which writes other SQL (sometimes executed by other SQL), it's natural to think quite carefully about how to write SQL.  As such we have some simple guidelines which support code readability, clarity and maintainability.  Of course rules are made to be broken so these are not always set in stone, however they hold in the vast majority of use-cases.

=== "Optimise for Readability" 
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
    Although it is possible to use e.g. `GROUP BY 1, 2, 3`to group based on ordinal position, this should be avoided as it negatively impacts readability: the reader will need to then scan up the page to identify the precise grouping columns in the query.  It is also less robust than grouping by specific columns as re-ordering the columns might produce inconsistent and erroneous results.     

## Column Naming Convention
=== "Case" 
    Columns are always in Camel Case (i.e. lower case, separated by underscores e.g. `video_views` or `media_id`).  If the column type is an array, they suffix `_array` is added to the column name.

=== "Aggregate Columns" 
    When a column is aggregated, in order to preserve the relationship in the flow profile the new name should be descriptive, but include the source name as a substring (e.g. `sum_video_views` or `total_video_views`).  This is so the columns can be related across flow stages using simple string comparisons. 

=== "Related Columns" 
    Similar to aggregate columns, to maintain the relationship when profiling across the entire flow the derived column name should contain the source column name (e.g. `media_id_deleted_flag` or `valid_video_views_flag`).

## Other Naming Conventions
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

[^1]: This approach is loosely based on the [Johnny.Decimal](https://johnnydecimal.com/) file organisation system, and is intended to be simple and flexible without being overly prescriptive.