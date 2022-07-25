# Reference Documentation


### Date Partitioned Tables
 Date partitioned tables are the foundation for efficient data transformation flows.  In date partitioned tables, the data is actually physically stored in sequential partitions, identified by date[^1].  This means that if you know the date of thepartition(s) you are looking for it is quick and cost-effective to access that data.  They eliminate the need for expensive and slow full-table-scans and help avoid repeating data processing unnecessarily.

They also enable optimised downstream performance in BI tools such as Data Studio.  If you set the date parameters in a Google Data Studio report to be a date-partitioned field then the data transfer between BigQuery and Data Studio is limited just to the partitions required, increasing responsiveness and improving the end-user experience. 

### Date Sharded Tables
Sharded tables are similar to partitioned tables, but with some key differences.
