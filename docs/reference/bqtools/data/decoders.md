These functions are used to decode common input data structures into BigQuery structured data columns.

# **`decode_pubsub_scheduled_query_data`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `decode_pubsub_scheduled_query_data`
_**ID**_ | `bqtools.[region].decode_pubsub_scheduled_query_data`
_**Description**_ | Decodes the JSON `data` column from Scheduled Query notifications sent to BigQuery via a PubSub BigQuery subscription into a STRUCT.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `data JSON`
_**Returns**_ | `data STRUCT`
_**Dependencies**_ | `None`

!!! info "execution: `decode_pubsub_scheduled_query_data`"
    === "EU"
        ```sql
        SELECT *
        EXCEPT(data),
        bqtools.eu.decode_pubsub_scheduled_query_data(data) AS data
        FROM [project_id].BQMANAGER.SCHEDULED_QUERY_LOGS
        ```

    === "US"
        ```sql
        SELECT *
        EXCEPT(data),
        bqtools.us.decode_pubsub_scheduled_query_data(data) AS data
        FROM [project_id].BQMANAGER.SCHEDULED_QUERY_LOGS
        ```

??? abstract "response schema: `decode_pubsub_scheduled_query_data`"

    | Column Path | Mode | Data Type |
    | --- | --- | --- |
    | `data.dataSourceId` | `NULLABLE` | `STRING` |	
    | `data.destinationDatasetId` | `NULLABLE` | `STRING` |		
    | `data.emailPreferences` | `NULLABLE` | `RECORD` |		
    | `data.emailPreferences.enableFailureEmail` | `NULLABLE` | `BOOLEAN` |		
    | `data.endTime` | `NULLABLE` | `TIMESTAMP` |	
    | `data.errorStatus` | `NULLABLE` | `RECORD` |		
    | `data.errorStatus.code` | `NULLABLE` | `INTEGER` |		
    | `data.errorStatus.message` | `NULLABLE` | `STRING` |	
    | `data.notificationPubsubTopic` | `NULLABLE` | `STRING` |		
    | `data.params` | `NULLABLE` | `RECORD` |		
    | `data.params.query` | `NULLABLE` | `STRING` |		
    | `data.runTime` | `NULLABLE` | `TIMESTAMP` |		
    | `data.scheduleTime` | `NULLABLE` | `TIMESTAMP` |		
    | `data.startTime` | `NULLABLE` | `TIMESTAMP` |		
    | `data.state` | `NULLABLE` | `STRING` |		
    | `data.updateTime` | `NULLABLE` | `TIMESTAMP` |		
    | `data.userId` | `NULLABLE` | `STRING` |		

# **`decode_pubsub_scheduled_query_attributes`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `decode_pubsub_scheduled_query_attributes`
_**ID**_ | `bqtools.[region].decode_pubsub_scheduled_query_attributes`
_**Description**_ | Decodes the JSON `attributes` column into a STRUCT from a Scheduled Query notification sent to BigQuery via a PubSub BigQuery subscription.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `attributes JSON`
_**Returns**_ | `attributes STRUCT`
_**Dependencies**_ | `None`

!!! info "execution: `decode_pubsub_scheduled_query_attributes`"
    === "EU"
        ```sql
        SELECT *
        EXCEPT(attributes),
        bqtools.eu.decode_pubsub_scheduled_query_attributes(attributes) AS attributes
        FROM [project_id].BQMANAGER.SCHEDULED_QUERY_LOGS
        ```

    === "US"
        ```sql
        SELECT *
        EXCEPT(attributes),
        bqtools.us.decode_pubsub_scheduled_query_attributes(attributes) AS attributes
        FROM [project_id].BQMANAGER.SCHEDULED_QUERY_LOGS
        ```

??? abstract "response schema: `decode_pubsub_scheduled_query_attributes`"

    | Column Path | Mode | Data Type |
    | --- | --- | --- |
    | `attributes.eventType` | `NULLABLE` | `STRING` |	
    | `attributes.payloadFormat` | `NULLABLE` | `STRING` |		