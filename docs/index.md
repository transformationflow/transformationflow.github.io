---
hide:
  - navigation
---
## What is Transformation Flow?
---
!!! tldr "Summary"
    Transformation Flow is a lightweight, open source framework for planning, developing, implementing, documenting, managing and monitoring SQL-based in-warehouse data transformations.  
---

## Motivation
The ability to build data transformations in SQL within a data warehouse and have the results immediately available to downstream tools, models and consumers is a modern data superpower, but with great power comes great responsibility.  

### Structure
Unfortunately the responsibility to write clean, clear, well-structured code is often overlooked, especially when the transformations begin life as ad-hoc data explorations.  This means that the resulting flows can quickly grow into complicated interconnected networks of datasets, tables and views, which can be extremely difficult to manage, debug and build upon.

### Simplicity
The objective of this framework is to enable rapid development of robust data transformations within the data warehouse, wherever you want to write your SQL, without requiring any additional tools or installation. It is designed to help humans convert their data transformation objectives into simple, well-structured code, which is quick to build and deploy, and easy to manage, monitor and debug.

## Benefits
Aside from the obvious benefits of rapid, clear, readable, consistent and maintainable code, using this approach ensures that the resulting code is structured consistently and benefits from standardised helper and profiling code from the start.  It helps with step-by-step testing to ensure that the data transformation flow stages work as expected, with the aim of preventing 'needle in a haystack-sizedmound of dirty spaghetti code' debugging endeavours.

### Conventions
Clear definition of the overall approach, target structure and naming conventions is a simple but extremely powerful starting point to start building consistency and robustness to data transformation flows. 

### Process
Simply ensuring that any data transformation activities begin with a plan of desired stages and outcomes is a remarkably simple yet powerful aspect of any creative framework.  This basic (non-concrete) plan then build the code outline for the transformation flow.

### Outlines
Simple outline builder functions enable code structure outlines to be quickly generated, along with supporting helper functions to data transformation profiling and debugging within the SQL development workflow.

### Functions
Common patterns are abstracted into powerful functions to transform human intent into sometimes verbose SQL structures.  By leveraging the information schema[^1], user defined functions (UDF) and scripting capabilities, these new functions enable dynamic development of patterns which would typically require hard-coding of column-names into human-error-prone structures such as long case statements or column name string concatenations.

### Management
In addition to the code becoming easier to reproduce and more portable (as you can simply apply the exact same function with a few argument changes to deploy transformations to different source data), this approach has built-in data lineage, and is structured to support testing, debugging and automatic documentation of transformation flows.

## Approach
The framework is intended to simplify and accelerate the workflow for translating human intent into robust, maintainable SQL code, leveraging the power of functional encapsulation to build SQL in real time from an extensible set of functions and their parameters.

### Workflow Simplification
Data workflows can be extremely complicated, requiring repeated context switching, multiple systems and large-scale data management, which impacts productivity by limiting the opportunity to get into a flow state for creative problem solving.  By keeping the entire workflow inside one platform and mode of thinking, the objective is to maximise creative productivity.

### Deployment
Simple deployment patterns are documented and supported with powerful functions to leverage powerful native functionality (e.g. external tables, partitioned & clustered tables and Google Cloud Storage data exports) to enable the end-to-end logical flows to be deployed in a data-efficient manner and to minimise ongoing query costs and maximise performance.

### Open Source Code
The code base is open source, with the source function code (including arguments and detailed descriptions) soon to be made available at [transformationflow/flowfunctions](https://github.com/transformationflow/flowfunctions).  Access to the `flowfunctions` BigQuery project and functions is currently in private beta, please contact [flowfunctions@transformationflow.io](mailto:flowfunctions@transformationflow.io) for access in your geography.

## Alternatives
This framework was born because the alternatives did not meet the requirements for our specific use-cases, where implementation of additional external tools would have added unneccesary complexity to a data transformation process which is completely manageable in pure SQL (plus additional innovstions in scripting, functions, and Data Definition Language).

For more complex situations where hundreds or thousands of interdependant transformations need to be developed, deployed, orchestrated and managed, or collaboration is required across a large number of people or teams, then it might make more sense to use an external tool.

### dbt
[dbt](https://www.getdbt.com/) is the industry standard tool for data transformation, leveraging Jinja templating inside SQL to enable analytics engineers to model data and deploy/manage data transformations.  It is written in Python.

### Dataform
[Dataform](https://dataform.co/) is a BigQuery-specific platform for transformation collaboration, using Javascript and SQLX (a templated SQL variant).


[^1]: the various [INFORMATION_SCHEMA](https://cloud.google.com/bigquery/docs/information-schema-intro) views contain metadata about datasets, columns, tables, views and other assetss