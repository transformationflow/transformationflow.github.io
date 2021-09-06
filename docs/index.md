# Transformation Flow
## What is Transformation Flow?
---
!!! tldr "Summary"
    Transformation Flow is a lightweight, open source framework for planning, developing, implementing, documenting, managing and monitoring SQL-based in-warehouse data transformations.  
---

## Motivation
The ability to build data transformations in SQL and have the results immediately available to downstream tools and consumers is a modern data superpower, but with great power comes great responsibility.  

### Structure
Unfortunately the responsibility to write clean, clear, well-structured code is often overlooked, especially when the transformations begin life as ad-hoc data explorations.  This means that the resulting flows can quickly grow into a complicated interconnected network of datasets, tables and views, which can be extremely difficult to manage and debug.

### Simplicity
The objective of this framework is to support development of data transformations within the data warehouse, without requiring any additional tools or installation. It is designed to help humans convert their data transformation objectives into simple, well-structured code, which is quick to build and deploy, and easy to manage, monitor and debug.

## Benefits
Aside from the obvious benefits of rapid, clear, readable, maintainable code, using this approach reduces the amount of code written by human significantly, and ensures that the resulting code is easier to manage in the future. 

### Functions
Common patterns are abstracted into powerful functions to transform human intent into sometimes verbose SQL structures.  By leveraging the information schema[^1] and scripting capabilities, these functions enable dynamic development of patterns which would typically require hard-coding of column-names into human error prone structures like long case statements or column string concatenations for hashing.

### Management
In addition to the code becoming easier to reproduce and more portable (as you can simply apply the exact same function with a few argument changes to deploy the entire flow to different source data), this approach has built-in data lineage, and is structured to support testing, debugging and automatic documentation of transformation flows.

## Approach
The framework is intended to simplify and accelerate the translation of human intent into SQL code, leveraging the power of functional encapsulation to build the SQL from an extensible set of functions and their arguments. 

### Open Source
It is open source, with the source function code (including arguments and detailed descriptions) available at [transformationflow/txflow](https://github.com/transformationflow/txflow), and readily available to any authenticated BigQuery user in the public `txflow` project[^2].

### Extensible
The functions are extensible in a couple of ways.  Firstly, since the transformation flows result in SQL deployed to views, this SQL can be directly edited as normal.  However the downside of this approach is that if you rebuilt from the transformation flow, the changes would be lost.

Any arbitrary SQL query can also be used as a flow stage using the `txflow.transform.query` function, which should cover any use case. For more complex and/or dynamic query requirements you can submit a pull request or raise an issue on the [transformationflow/txflow](https://github.com/transformationflow/txflow) repo and this can be added to the function set.

## Alternatives
This framework was born because the alternatives did not meet the day-to-day requirements for our specific use-cases, where implementation of additional external tools would have added unneccesary complexity to the data transformation process.

### SQL Code
For simple transformations, there is no reason to not write the SQL code directly in a single common table expression based view.  The intention is that these views can be parsed into the `sql_blocks` structure to leverage data profiling and monitoring functions on arbitrary human-written code. 

### Modern Tools
For more complex situations where hundreds or thousands of transformations need to be developed, deployed, orchestrated and managed, or collaboration is required across a large number of people or teams, then it might make more sense to use an external tool.

#### DBT
[DBT](https://www.getdbt.com/) is the industry standard tool for data transformation, leveraging Jinja templating inside SQL to enable analytics engineers to model data and deploy/manage data transformations.

#### Dataform
[Dataform](https://dataform.co/) is a BigQuery-specific platform for transformation collaboration, using Javascript and SQLX (a templated SQL variant).

### Legacy Tools
There are also a number of legacy tools in this space, however they are typically tailored towards the enterprise and come with complex sales processes and high licence fees.  

[^1]: the various [INFORMATION_SCHEMA](https://cloud.google.com/bigquery/docs/information-schema-intro) views contain metadata about datasets, columns, tables, views and other assets
[^2]: `txflow` is an abbreviation for `transformationflow`
