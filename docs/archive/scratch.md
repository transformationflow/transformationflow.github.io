## Motivation
The ability to build data transformations in SQL within a data warehouse and have the results immediately available to downstream tools, models and consumers is a modern data superpower, but with great power comes great responsibility.  








### Structure
Unfortunately the responsibility to write clean, clear, well-structured code is often overlooked, especially when the transformations begin life as ad-hoc data explorations.  This means that the resulting flows can quickly grow into complicated interconnected networks of datasets, tables and views, which can be extremely difficult to manage, debug and build upon.


### Benefits
Aside from the obvious benefits of rapid, clear, readable, robust and maintainable code, using this approach ensures that the resulting code is structured consistently and benefits from standardised helper and profiling functions from the start, within the SQL query editor.  It helps with step-by-step testing to ensure that the data transformation flow stages work as expected as built, with the aim of preventing 'needle in a haystack-sized mound of dirty spaghetti code' data debugging endeavours.

## Conventions
Clear definition of the overall approach, target structure and naming conventions is a simple but extremely powerful starting point to start building consistency and robustness to data transformation flows. 

## Process
Simply ensuring that any data transformation activities begin with clear objectives and a sequential plan of stages which lead to those objectives is a remarkably simple yet powerful characteristic of any creative framework.  This basic plan then builds the initial, extensible code outline for the transformation flow.

## Functions
Common patterns are abstracted into powerful functions to transform human intent into sometimes verbose SQL structures.  By leveraging the information schema[^1], user defined functions (UDF) and scripting capabilities, these new functions enable dynamic development of patterns which would typically more error-prone human interaction.  

## Management
In addition to the code becoming easier to reproduce and more portable (as you can simply apply the exact same function with a few argument changes to deploy transformations to different source data), this approach has built-in data lineage, and is structured to support testing, debugging and automatic documentation of transformation flows.

## Approach
The framework is intended to simplify and accelerate the workflow for translating human intent into robust, maintainable SQL code, leveraging the power of functional encapsulation to build SQL in real time from an extensible set of functions and their parameters.

## Workflow Simplification
Data workflows can be extremely complicated, requiring repeated context switching, multiple systems and large-scale data management, which impacts productivity by limiting the opportunity to get into a flow state for creative problem solving.  By keeping the entire workflow inside one platform and mode of thinking, the objective is to maximise creative productivity and enjoyment of the process.

## Deployment
Simple deployment patterns are documented and supported with powerful functions to leverage powerful native functionality (e.g. external tables, partitioned & clustered tables and Google Cloud Storage data exports) to enable the end-to-end logical flows to be deployed in a data-efficient manner and to minimise ongoing query costs and maximise performance.

## Open Source Code
The core code base is intended to be open source, but access to the `flowfunctions` BigQuery library (and also the EU-specific library mirror `flowfunctionseu`) is currently in private beta.  Please contact [flowfunctions@transformationflow.io](mailto:flowfunctions@transformationflow.io) for access in your geography.

