# Event Profiling

## Context
Google Analytics 4 event data has a schema which can be extremely difficult to work with, even for SQL experts.  All automatic and custom parameters and properties (e.g. `event_params`, `user_properties`) are returned in `ARRAYS` of complex key-value `STRUCT` pairs, which require prior knowledge of the name _and_ the data type of the paremeters and properties.  

This is further complicated by the fact that the automatic parameters can change over time, and the data types are not always consistent.  

??? info "example: parameter data type inconsistency"
    An example here is the `ga_session_id` which is typically an `INT64`, but which we have observed in a number of streams as a `STRING`. This means that extracting the `INT64` value  using the following example query - which should return all raw event_data plus the `ga_session_id` as a separate column - would lose data whenever the value was returned as a `STRING`.

    ```sql
    SELECT *,
    (SELECT event_param.value.int_value FROM UNNEST(event_params) AS event_param WHERE event_param.key = 'ga_session_id')
    FROM `project_id.analytics_##########.events_2023*` 
    ```

    This would result in under-reporting of session counts.

## Approach
### Profiling
The approach we take, in order to enable users to remodel their raw data into a useable structure, is to run a set of profiler functions over the raw data, in order to determine:

- `event_name`: unique values
- `event_params`: unique names, data types and data type occurrences
- `user_properties`: unique names, data types and data type occurences

Neither the `ecommerce` nor the `items` are included in the profiler at this stage.

### Data Type Coercion
In order to avoid potential data loss due to data type inconsistency, we leverage a number of simple functions to extract _all_ data from a value `STRUCT`.  The function used to extract data from each column is determined by the `event_profile` derived in the profiling stage.  The potential data types returned in the `event_params` and `user_properties` `value` columns are:

Value Column | Data Type |  Coercion Logic 
--- | --- | --- 
`value.string_value` |  `STRING` | Any data type can be reliably coerced into a `STRING`
`value.int_value` | `INT64` | Neither `FLOAT64` nor `STRING` values can be reliably coerced into an `INT64`
`value.float_value` | `FLOAT64` | Only an `INT64` value can be reliably coerced into a `FLOAT64`
`value.double_value` | `FLOAT64` | Only an `INT64` value can be reliably coerced into a `FLOAT64`

### Decoding Expressions
Once the data has been profiled and we understand the event characteristics, we can build the SQL required to decode the complex structures into the desired output structures.  For the `event_name`, `event_params` and `user_properties` the strategies employed are:

**`event_name`**

_**Attribute**_ | Value
--- | ---
_**Column Name**_ | `event_name`
_**Column Schema**_ | `STRING`
_**Default Values**_ | `click`, `first_visit`, `page_view`, `scroll`, `session_start`, `user_engagement`, `purchase`
_**Challenges**_ | While each event has a single, non-null `event_name`, this structure can be difficult to use in downstream reporting tools.  For example, in Looker Studio it is simple to display one of these metrics (e.g. `page_view`) as a Scorecard by adding a chart filter for e.g. `event_name = 'page_view'` and adding the `records` metric to the scorecard.  However if you wanted to plot multiple `event_name` metrics on a time series this would be impossible as the filters required would cancel each other out. <br><br>It _is_ possible to get around this by creating calculated fields for each filter, but it is laborious, adds unnecessary complexity and could also potentially introduce human errors as formula errors are common.
_**Approach**_ | The original `event_name` column is retained in the data as it can be useful for a variety of use-cases.  However an additional `STRUCT` flag column `event_count` is included, with a flag (`1` or `NULL`) column for each `event_name` value.  This means that default events like `page_view` become their own metric `event_count.page_view`, which can be treated as a normal metrics, allowing usage of normal mathematical functions.  <br><br>Custom events will also be included as their own columns within the `event_count` `STRUCT`, and plotting metrics against each other on a single chart is now trivial 
_**Output Schema**_ | `event_name STRING, event_count STRUCT<event_name_1 STRING, event_name_2 STRING, ... event_name_n STRING>`

???+ info "`event_count.conversions`"
    Conversions are not explicitly identified in BigQuery GA4 events, however we add an additional `event_count.conversions` column in the `event_count` output `STRUCT` to enable them to be used as a metric. The default logic for this is aligned to the [Google Analytics 4 definition](https://support.google.com/analytics/answer/13128484?hl=en&ref_topic=10313214&sjid=3989782183447547548-EU), and includes all events with `event_name = 'purchase'`.  This can be updated if additional events are marked as conversions, however it will not happen automatically.

**`event_params`**

_**Attribute**_ | Value
--- | ---
_**Column Name**_ | `event_params`
_**Column Schema**_ | `event_params ARRAY<STRUCT<key STRING, value STRUCT<string_value STRING, int_value INT64, float_value FLOAT64, double_value FLOAT64>>>`
_**Default Values**_ | **`event_params.key`:** `anonymize_ip`, `campaign`, `content`, `dclid`, `debug_mode`, `engaged_session_event`, `engagement_time_msec`, `entrances`, `ga_session_id`, `ga_session_number`, `gclid`, `ignore_referrer`, `language`, `link_classes`, `link_domain`, `link_id`, `link_url`, `medium`, `outbound`, `page_location` , `page_referrer`, `page_title`, `parent_term`, `percent_scrolled`, `screen_resolution`, `session_engaged`, `source`, `term`
_**Challenges**_ | Extracting values from this complex `ARRAY` column requires previous knowledge of both the `event_params.key` to be extracted and the data type, as well as using the `UNNEST` operator to access array elements, which is a complicated syntax to master, e.g. <br>```(SELECT event_param.value.int_value FROM UNNEST(event_params) AS event_param WHERE event_param.key = 'ga_session_id')```<br> Using this structure in a downstream reporting tool is near-impossible as it requires complex calculated fields for every possible key expected in the data.
_**Approach**_ | The decoding expression for the `event_params` column is generated programmatically from the `event_profile` derived in the profiling stage, and leverages a set of extract functions (`extract_event_params_double_value`, `extract_event_params_float_value`, `extract_event_params_int_value`, `extract_event_params_string_value`) to avoid data loss due to type inconsistencies.

**`user_properties`**

_**Attribute**_ | Value
--- | ---
_**Column Name**_ | `user_properties`
_**Column Schema**_ | `user_properties ARRAY<STRUCT<key STRING, value STRUCT<string_value STRING, int_value INT64, float_value FLOAT64, double_value FLOAT64, set_timestamp_micros INT64>>>`
_**Default Values**_ | None
_**Challenges**_ | Extracting values from this complex `ARRAY` column requires previous knowledge of both the `user_properties.key` to be extracted and the data type, as well as using the `UNNEST` operator to access array elements, which is a complicated syntax to master, e.g. <br>```(SELECT user_property.value.int_value FROM UNNEST(user_properties) AS user_property WHERE user_property.key = 'ga_session_id')```<br> Using this structure in a downstream reporting tool is near-impossible as it requires complex calculated fields for every possible key expected in the data.
_**Approach**_ | The decoding expression for the `user_properties` column is generated programmatically from the `event_profile` derived in the profiling stage, and leverages a set of extract functions (`extract_user_properties_double_value`, `extract_user_properties_float_value`, `extract_user_properties_int_value`, `extract_user_properties_string_value`) to avoid data loss due to type inconsistencies. 

### Base Query
We start with a base query, which is structured as a sequence of common table expressions (explicitly named SQL-defined logical steps), stored as a `cte_profile` object in a `profile_table`.  We then use the `update_cte_sql` from our `bqtools` library to update the `decode_event_names`, `decode_event_params` and `decode_user_properties` ctes with the programmatically generated custom SQL. This results in a custom decoder query, specific to the event stream profiled using the `profile_events` function.

### Decoder Table Function
This custom decoder query is then deployed in the GA4 BigQuery dataset as a date-paramterised Table Function called `events`, which can be efficiently queried directly by specifying a `start_date` and `end_date` `DATE` parameter, e.g. for the past 30 days

```sql
SELECT * 
FROM `[project_id].analytics_########.events`(CURRENT_DATE - 30, CURRENT_DATE)
```

It can also be queried directly from Looker Studio with report date parameters, using the following code sytnax to parse the `DS_START_DATE` and `DS_END_DATE` parameters into SQL `DATE`` values:

```sql
SELECT * 
FROM `[project_id].analytics_########.events`(
PARSE_DATE("%Y%m%d", @DS_START_DATE), 
PARSE_DATE("%Y%m%d", @DS_END_DATE));
```

For event streams which have either a high query load, a large amount of data or both, we can use `bqtools` to build and periodically update a date-partitioned table, to which downstream tools can connect directly.  This will optimise performance and cost, but requires a scheduled query to keep updating with the latest partitions.

### Multi-Property Decoding
It is also possible to 'merge' multiple `event_profile` objects to build a universal decoder, which will result in a perfectly aligned schema across multiple GA4 properties regardless of discrepencies seen in actual observed data.  This means that the downstream data can be integrated trivially.  
