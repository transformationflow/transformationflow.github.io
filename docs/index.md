# What is Transformation Flow?
---
!!! tldr "Summary"
    Transformation Flow is a framework for planning, developing, implementing, optimising, documenting, managing and monitoring SQL-based data transformation and augmentation in Google BigQuery.  
---

## Motivation
Google BigQuery is an almost infinitely powerful foundation for any kind of analytics work, and it can be used in virtually unbounded ways.  This makes it difficult to structure optimally, especially for those new to the platform (or new to working with data).  

After years of experience refining and optimising ways of working within BigQuery with a large number of clients and types of data, while the platform capabilities have evolved in parallel, this framework is the codification of this evolved methodology.

The core motivation for developing and sharing this are:

- **Accessibility** - lowering the barriers to entry for anybody wanting to access these modern data superpowers
- **Simplicity** - helping people achieve complex outcomes using logical, composable structures and resources, with minimal dependencies on external tools
- **Extensibility** - leveraging non-SQL language capabilities and the wider API ecosystem to extend what is possible in BigQuery

There are third party solutions to manage different roles in (e.g. dbt for transformation and documentation, Airflow for orchestration, Dataflow for augmentation... etc!), however all of this can be achieved within BigQuery, without the need to learn, configure and manage multiple tools. 

## Situation
This is only possible due to the technical advancements in BigQuery, which have evolved it from a 'data warehouse' (i.e. a place to store and retrieve data) to a 'data platform' (i.e. a foundational enabling technology), if you know how to use it.

The most important features which we use are:
- **Views** - enabling logic to be decomposed into a sequence of logical, testable (but ephemeral) steps, without requiring data to be continually re-created.
- **Partitions** - enabling subsets of data to be queried and processed, reducing development and testing time, and supporting cost optimisation  
- **Functions** - enabling complex computations to be encapsulated into functional, reuseable code, leveraging SQL and Javascript functionality
- **Routines** - enabling complex logic to be defined and executed, using control flows and complex data structures
- **Information Schemas** - enabling easy visibility and use of metadata to build robust logical structures and automation 
- **Remote Functions** - enabling access to external APIs at scale via (serverless) Python Cloud Functions  

These resources, in combination with specific naming conventions and taxonomies, enable us to use a generalised core methodology to solve specific data-related problems across almost any any field. 

## Solution
This framework uses a number of different components under the hood, however in its simplest form there are only two categories:

- **Methodology** - specific ways of thinking about, naming and working with BigQuery (and Google Cloud) resources 
- **Functions** - the `flowfunctions` functional extensions which enable users to execute complex tasks in simple workflows.  Their precise implementation varies depending on context and objectives.

The solution is fundamentally designed to be BigQuery-centric, enabling development, testing, orchestration and execution without ever needing to leave the BigQuery Console. It can also be used by attached tools, so wherever you can run BigQuery SQL (via e.g. connected notebooks, dashboards or IDEs), you can use `flowfunctions`.

## Execution
Access to the documentation and functions is free and open to all, however due to authentication requirements access to some functions will require an invite to the flowfunctions permission Google Group.  Please email info@transformationflow.io for information/access.

The flowfunctions project is a multi-region US-based project, so functions are called using the `flowfunctions.function_set.function_name`syntax if you are operating on US-based data.  For EU-based users, the library is mirrored to the `flowfunctionseu` project.





