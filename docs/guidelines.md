# Transformation Flow Guidelines
These guidelines are consistently applied when we build data transformation flows, and should give a clear understanding on how we think about and work with data.

## Data Ingestion
Moving data around consistently, reliably and at scale is a complex problem to solve. However there are many potential solutions, especially when working with BigQuery.  

### Native Services
Using native services should be the preference here, whether it is direct integration with a Google product or
direct connection from an external supplier.  

### External Tables
External tables can be a very low-maintenance solution to data ingestion requirements, as data can be directly queried from Google Cloud Storage or Amazon S3 buckets without requiring loading of the data.  Files with the schema included (i.e. Parquet or Avro) are preferable to CSV or JSON files.

### 3rd Party Services
3rd Party Services are essential when you need to integrate data sources without native connectors. We recommend that you pick a single service, and one where your costs do not scale with the volume of data ingested.  Currently we recommend:

- [Airbyte](https://airbyte.com/): If you are comfortable deploying and maintaining your own instance on a virtual machine.
- [Dataddo](https://www.dataddo.com/): If you are not.

Both are extensible, however Dataddo will build new connectors for you at no cost.  Airbyte will require more technical input (predominantly Python/Java), but has a friendly, active and fast-growing community.

### Custom Development
This is almost always a bad idea. Continue down this path at your peril... then use a 3rd party specialist or platform (see above) to do the job properly.

## Dataset Segregation
Segregating your datasets for a given source is an extremely important aspect of Data Warehouse organisation.  We recommend:


### Inbound Data
### Data Transformation
### Outbound Data

## Table/View Naming Conventions
### Flow Stage Index and Title
### Suffix vs. Prefix
### Table Types

## Column Naming Convention
### Snake Case
### Aggregate Column Naming
### Related Column Naming

## Physical Data Configuration
### Partitioning
### Clustering

## Cost Optimisation
### Latest Partition Operations
### Dynamic Query Sections

## SQL Style
When you spend time writing SQL which writes other SQL (sometimes executed by other SQL), it's natural to think quite carefully about how to write SQL!  As such we have some simple guidelines which support code readability, clarity and maintainability.  Of course rules are made to be broken, so these are not always set in stone, however they hold in the vast majority of use-cases.

### Always Use Common Table Expressions
### Limit Subquery Usage
### Capitalise Keywords
### Left-Justify Keywords
### Do Not Indent






