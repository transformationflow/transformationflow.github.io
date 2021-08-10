# Transformation Flow
## What is Transformation Flow?
---
!!! tldr "Overview"
    Transformation Flow is a lightweight, open source framework for planning, developing, implementing, documenting, managing and monitoring SQL-based in-warehouse data transformations.  
---

## Who is it for?
Transformation Flow is for any human who wants to work with data in an efficient, scaleable and repeatable manner.  It is fundamentally SQL-based, but no prior experience of SQL is required or expected as the methodology and worked examples are designed to cover every concept and motivation.  SQL beginners should find that it covers the core language structure and commands required to get started (and productive) quickly; SQL experts should find that it accelerates their workflow due to the abstraction of common and complex patterns into powerful functions.  

## Why does it exist?
The world of data is changing so rapidly, as new tools, capabilities (and even job titles) seem to appear every week and the core data professions of Data Engineering, Data Analytics Engineering, Data Analytics and Data Science mature and evolve at breakneck speed.

However as the technology and capability frontiers accelerate towards the level of automation, scalability and professional standards commonly seen in the software engineering world, it is easy to forget about those trying to cross the other frontier... people, teams and organizations looking to move from their previous world of spreadsheets and manual human/data interaction to the new world of accessible, professional, world-class data tools.

Transformation Flow is designed to support any individual or team, and to help them leverage modern analytics tools quickly and professionally for maximum data impact in the minimum time.

## Why Open Source?
Open source means that the underlying code for Transformation Flow functions is shared freely, and can be copied, updated and used for any purpose.  It also means that people beyond the initial developers can collaborate and contribute to the code base to help it evolve and grow to meet their specific needs.  

As the Transformation Flow core is open source, access to the functions and underlying code in the `txflow` and `txflowutils` function libraries is free and available to any user logged into BigQuery.

In the field of data, open source is becoming the de-facto standard for the modern analytics stack.  You could configure [Airbyte](https://airbyte.io) for data ingestion, [DBT](https://www.getdbt.com/) for data transformation and [Superset](https://superset.apache.org/) for data exploration, visualisation and alerting, and this 'best-of-breed' stack is completely open-source.  This means that it can be deployed, tested and operated free of charge (except for any cloud provider fees and the the human effort required to manage the infrastructure, deployments and integrations).  Open source is about leveraging the power of human collaboration at scale.

In fact, the only part of the modern analytics stack which is not typically open source is the Cloud Data Warehouse (CDW), the fundamental component where data storage and query takes place.  Transformation Flow is currently built to function within Google Cloud Platform, using BigQuery as the Cloud Data Warehouse.

## Why Google Cloud Platform and BigQuery?
There are three main reasons that Transformation Flow uses Google BigQuery as the Cloud Data Warehouse for the backbone of the framework: accessibility, capability and the integrated ecosystem.  

### 1. Accessibility
Google BigQuery is the only Cloud Data Warehouse which abstracts away _all_ infrastructure, so users do not need to know technical details like how many clusters they might need.  All you need is a Google ID and BigQuery can even be operated in sandbox mode free of charge, has a free tier for many operations and also gives a free $300 credit for new users.  You simply log into [BigQuery](https://console.cloud.google.com/bigquery) and you can start writing SQL queries and seeing the results in minutes.

The ease of deploying and sharing functions within BigQuery is also a major plus point.  The ability to develop and test a function, and then publish by simply sharing the dataset means that we can rapidly deploy new functionality without a time consuming deployment process. 

It is also very simple to interact with BigQuery via the API and client libraries, which has enabled us to develop the Transformation Flow Console to further speed up typical data transformation development, deployment and monitoring workflows. 

### 2. Capability 
BigQuery capabilities to process and transform any sized dataset from a simple spreadsheet to petabyte scale are incredibly powerful and constantly extending.  In addition to typical SQL-based query actions, administration activities can be executed via the same interface using [Data Definition Language](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language) (DDL) or [Data Manipulation Language](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-definition-language) (DML), Machine Learning models can be developed in-warehouse using [BigQuery ML](https://cloud.google.com/bigquery-ml/docs/introduction) and Geographic Analysis can be undertaken using [BigQuery GIS](https://cloud.google.com/bigquery/docs/gis-intro).  The introduction of [scripting](https://cloud.google.com/blog/products/data-analytics/command-and-control-now-easier-in-bigquery-with-scripting-and-stored-procedures) in BigQuery added another dimension by enabling development of functions in SQL and Javascript, further extending custom functionality, which Tranformation Flow uses in the `txflow` and `txflowutil` function libraries.

Forrester rated BigQuery as the Leader in their [Q1-2021 Cloud Data Warehousing Report](https://cloud.google.com/forrester-wave-cloud-data-warehouse-2021), and as BigQuery continues to innovate and grow we are confident in our choice to base Transformation Flow within this ecosystem.

### 3. Integrated Ecosystem
BigQuery is deeply integrated with other key components of Google Cloud Platform and Google Workplace, which makes a number of different powerful workflows trivial to set up and use:

#### Google Sheets 

#### Google Data Studio

#### Google Cloud Storage 



