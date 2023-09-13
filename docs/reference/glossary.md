The following definitions are used consistently across all functions and development work.  These definitions may override some of the standard Google definitions which can be inconsistent across different system functions, views and user interfaces.

Term | Data Type | Description | Key Functions
--- | --- | --- | --- 
`project_id` | `STRING` | The globally unique identifier for each [project](https://cloud.google.com/resource-manager/docs/creating-managing-projects). | `bqtools.[region].parse_resource_id(resource_id).project_id`
`dataset_name` | `STRING` | The name for each [dataset](https://cloud.google.com/bigquery/docs/datasets), unique in each project. | `bqtools.[region].parse_resource_id(resource_id).dataset_name`
`dataset_id` | `STRING` | The globally unique identifier for each dataset, composed as `project_id.dataset_name`. | `bqtools.[region].parse_resource_id(resource_id).dataset_id`
`resource_name`  | `STRING` | The single name for each resource (tables, views, functions, table functions or procedures). | `bqtools.[region].parse_resource_id(resource_id).resource_name`
`resource_id` | `STRING` | The globally unique identifier for individual tables, views, functions, table functions or procedures, composed as `project_id.dataset_name.resource_name`. | `bqtools.[region].parse_resource_id(resource_id)`
`date_id` | `STRING` | A `STRING` representantion of a date in the format `YYYYMMDD`. | `SET date_id = PARSE_DATE('%Y%m%d', date)`, <br>`SET date = FORMAT_DATE('%Y%m%d', date_id)`
`shard_id` | `STRING` |  A `STRING` representantion of a shard date in the format `YYYYMMDD`. | [Sharded Tables](https://transformationflow.io/reference/bqtools/sharding/)
`shard_date` | `DATE` |  A `DATE` representantion of a shard date. | [Sharded Tables](https://transformationflow.io/reference/bqtools/sharding/)
`partition_id` | `STRING` |  A `STRING` representantion of a partition date in the format `YYYYMMDD`. | [Partitioned Tables](https://transformationflow.io/reference/bqtools/partitioning/)
`partition_date` | `DATE` |  A `DATE` representantion of a partition date. | [Partitioned Tables](https://transformationflow.io/reference/bqtools/partitioning/)

