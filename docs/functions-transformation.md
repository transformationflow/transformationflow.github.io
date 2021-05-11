# Data Transformation Functions Overview

## JSON Extraction
BigQuery can extract data from JSON stored in STRING fields and restructure it into scalar or array columns using [JSON functions in Standard SQL
](https://cloud.google.com/bigquery/docs/reference/standard-sql/json_functions).  

However, extraction of the data requires inspection of the exact structure and manually entering the exact path for each element, in addition to whether it is a scalar (single) value or array of elements.

These utilities automate the inspection and enable users to automatically and systematically extract JSON into native BigQuery data structures  

### `txflow.transform.extract_json_array`
