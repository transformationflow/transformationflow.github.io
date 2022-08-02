## Deployment
=== "Inbound Partitioned Tables" 
    Partitioned tables are preferred for inbound data, as when performing QA operations it grreatly improves query speed and reduces cost.  If 3rd party suppliers do not directly support this, it is worth testing if they can load data into a partitioned table as this is often the case.

=== "Intermediate Tables" 
    For complex flows (or flows where intermediate stages are referenced by a number of downstream stages) it can be beneficial for development, debugging and performance to build intermediate aggregated tables, which will be refreshed using a custom function in the `_admin` dataset.

=== "Outbound Partitioned Tables" 
    Outbound tables will almost always be partitioned, either on the ingestion date or a specific date field.  This results in cost and performance benefits, although there are some complexities to be addressed in order to leverage  full optimisation from partition subset operations.

=== "Partition Subset Operations" 
    One of the major benfits of partitioned tables the fact that you can operate on a specific subset of partitions and therefore limit unnecessary reprocessing of data.  There are some deployment complexities as to leverage this effiency you technically need to hard-code the partition(s) you want to reference, however you can deploy a flow using this pattern using some of our core `flowfunctions`.

=== "Scheduled Scripts" 
    Automation activities leverage scripting in BigQuery, packaging commands up into executable functions.  These functions can then be run ad-hoc in the console e.g. `CALL project.dataset_admin.refresh_tables()`or by scheduling this function call using BigQuery Scheduled Functions.  Scheduled functions should not be used for complex logic or SQL as all SQL should be in BigQuery for governance purposes.

---