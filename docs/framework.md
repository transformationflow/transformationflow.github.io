---
hide:
  - navigation
---
# Overview
## Tables and Views
### External Tables
### Partitioned Tables
### Sharded Tables
## Flow Structure
### Inbound Tables

## Transformation Types

## View Structure
```
WITH inbound_data AS (
SELECT * FROM project.dataset.table
)


SELECT * FROM inbound_data
```

### Common Table Expressions




Write an interesting introduction here

## Logical Flow vs. Deployment
Conceptually, there are two distinct ways of thinking about transformation flows:

- Logical Transformation Flow
- Transformation Flow Deployment

### Logical Transformation Flow

This is the SQL-based logic which transforms the structure and content of data from the inbound data source to the outbound data, to be consumed by downstream tools.  This is where the pure logic is defined, built and tested as a dependent set of BigQuery views #bq/view.

### Transformation Flow Deployment
In the simplest case with small inbound datasets, you might not require any deployment logic at all.



