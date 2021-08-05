# Transformation Flow

## Overview
### What is Transformation Flow?
Transformation Flow is a lightweight, opinionated framework for planning, implementing, managing and monitoring in-warehouse data transformations.  There are three key components:

#### Transformation Flow Methodology
This is the overarching approach to designing data transformations.  This is a set of guidelines and a framework for thinking about the structure and characteristics of data in order to build transformations which achieve their specific objectives.  This methodology has been refined over decades of working in the evolving data environment, and is aligned to the modern data analytics stack and ways of thinking.

The guiding principles, structure and methodology are detailed in [link to overview-methodology], but it is essentially arranged around the following core principals:

---
!!! tldr "Transformation Flow Core Principle #1"
    Transformation Flows are composed of one or more individual transformation stages, with each stage aiming to achieve one and only one specifc transformation action to the input data.  These flow stages are then composed into sequences of logical steps, so at every point the data shape and structure can be checked, tested and monitored via manual or automated methods.
---

---
!!! tldr "Transformation Flow Core Principle #2"
    Humans are great at abstract thought and introducing unpredictable errors into anything they touch.  Every copy/paste or find/replace represents another potential source of human error which should be avoided at all costs. 
---

#### Transformation Flow Functions
This methodology is supported by a set of functions to enable abstraction to an appropriate level for human interaction.  The approach here is to abstract individual transformation objectives where the SQL implementation would be either:

- **Predictable but verbose:**
    - If a user needs to - for example - apply _the same_ data transformation to a number of different columns, SQL requires the user to explicitly identify each column by name and repeat the transformation code for each column, potentially introducing human error if the manual repetition is not completed _perfectly_.  Transformation Flow functions enable the user to pass an explicit list of column names to a specific function and have the verbose SQL generated immediately and perfectly.

- **Explicit instead of implicit:**
    - Transformation Flow functions are sequential and composable by design.  This means that it is trivial to apply a transformation to a number of different columns where the column_name adheres to a specific pattern (e.g. every column suffixed with `_lifetime`).  In this case the user can call a Transformation Flow function which outputs a list of matching column names, and pass this list as a variable to the subsequent data transformation flow stage.

- **A predictable, multi-step process**
    - Functions are also composable into objective-focused groupings, which can output a sequence of common table expressions instead of a single expression.  This means that a user can call a function with the objective of e.g. "filter for the final row on each `ingest_date` for each `id`", and the Transformation Flow function will build sequential common table expressions to:
        - Add a `row_number` column to each row, partitioned by `ingest_date` and `id`, ordered by most recent `ingest_date` (i.e. descending)
        - Filter for `row_number` = 1 (most recent row per day)
        - Column filter to remove the `row_number`

[move to examples]

Functions are built to leverage core BigQuery functions, while simplifying interaction without (where possible) limiting options.

#### Transformation Flow Console
Whilst functions are the foundational building blocks of any data transformation, limitations in the implementation of routines in BigQuery (i.e. lack of autocomplete for custom routines, no support for keyword arguments or default argument values) can still make development slower than is desirable.  For this reason we have developed the Transformation Flow Console, which performs a number of higher-level functions and gives the user some extremely useful capabilities in profiling data, rapidly building and testing transformation flows and setting up the infrastructure to monitor data flows across the entire BigQuery project.  

Although all of the data transformation functions are defined in SQL within the `txflow` BigQuery project, building a User Interface enables us to benefit from some more advanced profiling and visualisation tools. This enables the developer to stay within a single tool and minimised time lost due to context switching and distraction.

The console is organised in the following structure:

##### Transformation Flow Builder




##### Data Profiling
There are a number of critical elements to profiling data, which enable data engineers, analytics engineers, analysts and scientists to better understand both the underlying data, the relationships between different views and tables, and the way data changes through each Transformration Flow.

###### Single Table/View Column-level Profiling
###### Table and View Relationships
###### Data Transformation Flows
###### View Health Analysis
###### Table/View Usage Statistics




##### Data Transformation
##### Data Monitoring




### What is a data transformation?
A data transformation is a sequence of one or more SQL statements which take data as an input and results in data as an output.

## Transformation Flow Data Philosophy
### Modularity
By considering each transformation flow stage as the minimum modular, sequential action,

### Simplicity

### What problem does it solve?


### Why SQL?
SQL is the de-facto language of choice for data engineers, data analysts and data scientists.  It is far from a perfect language, however many of the objections commonly leveled at the language can be mitigated by leveraging scripting and encapsulating logic into 
 
SQL is the common language for anybody who works with data. It is an extremely powerful if imperfect language, and its capability and widespread adoption makes it the default choice for any data transformation application.

### SQL shortcomings
However SQL is not perfect. It can be extremely verbose, which makes it difficult to write code without manually copy-pasting sections and requiring manual human update of references. This unwelcome introduction of human interference is a potential source of errors, which can then be difficult to detect in the resulting code.

### The Zen of SQL
### Where does it fit into the modern analytics stack?

## Transformation Flow Framework
### SQL Blocks

### Dependencies
### Capabilities
#### Conciseness
#### Flow Profiling
#### Flow Monitoring
#### Dependency Tracking


## Transformation Flow Resources
### Functions
Functions are the core building blocks of a Transformation Flow, which enable common data preparation and transformation steps to be abstracted to a specific objective instead of requiring verbose and potentially brittle hand-written SQL code.

### Cloud Functions
### Console




## Transformation Flow Stages
### Overview
### Ingestion
### Preparation
### Transformation
### Export
### Monitoring
