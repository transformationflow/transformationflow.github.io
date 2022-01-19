# Multiple CSV Data Sources from a Single GCS Bucket
Despite the fact that CSV is a less robust file format for data than Parquet or Avro - which have the schema description explicitly defined as part of the file -  it is still an extremely common format for data exchange.  This guide shows how to build a generic, robust dynamic data source from a bucket containing CSV files.

## Context

We will use a Google Cloud Storage bucket as the data source for this transformation flow. The situation we are addressing is one where:

1. there are a twenty one different file types in a single bucket
2. the identifier for a file is a string contained in the file name, but not at the beginning or end

Point 2 here is very important as if the report identifier was as the end or the beginning of the filename then it would be simple to build the 21 different data source tables using single wildcards in the EXTERNAL TABLE definition.  Unfortunately it is not possible to use more than one wildcard, so we need to specify the exact source URIs in the EXTERNAL TABLE definition.

This approach also includes a step to define the seed schema from an existing table. 

## Overview
This approach is designed to deal with cases where:

- additional columns might be added to the end of file schemas
- files might include errors

### Preparation
The initial steps we will take to build these data sources is therefore:

1. Build a GCS inventory view and table
2. Build a set of single URI seed schema tables
3. For each report type:
	1. Get the schema of the schema table and build a safe schema (all STRING data types)
	2. Get all source URIs
	3. CREATE the EXTERNAL TABLE from all source URIs and the safe schema

### Execution
Once the tables are built, the daily update requires the following steps to be scheduled:

1. UPDATE the GCS inventory table from the view
2. For each report type:
	1. Get the schema of the destination table and build a schema string
	2. Get all source URIs
	3. REPLACE the EXTERNAL TABLE from all source URIs and the schema string

## Monitoring
Scheduled queries can be configured to send a message to PubSub with the query and the status. We will use this to monitor the query runs via a single Cloud Function, which simple streams the JSON into a BigQuery table for downstream decoding.  Unfortunately the payload does not contain the Scheduled Query name, so we will need to include this as a commented JSON object as the first line of the header.

This is then decoded into a single table, which we can then link to a monitoring dashboard or scheduled notification.