
# Data Transformation Functions
These functions form the core of the framework and aim to automate and simplify common data transformation patterns.

## Function group: profile | Function name: get_data_type_profile
Description of the function group: profile and function name: get_data_type_profile

### `txflow.profile.get_data_type_profile`
                    
|function_group|function_name|function_output|description|
| :--- | :--- | :--- | :--- |
|`profile`|`get_data_type_profile`|e.g. VIEW|Get potential data types for each column in a dataset|

#### Call Syntax
```sql 
 CALL txflow.profile.get_data_type_profile( 
        'sql_block_name__STRING'
        
'sql_blocks__ARRAY<STRUCT<name STRING, code STRING, dependencies ARRAY<STRING>>>'
        'source_ref__STRING'
)
```                    
#### Arguments

|argument|datatype|description|
| :--- | :--- | :--- |
|`sql_block_name`|`STRING`|`TODO: Here the relevant description pending to add`|
|`sql_blocks`|`ARRAY<STRUCT<name STRING, code STRING, dependencies ARRAY<STRING>>>`|`TODO: Here the relevant description pending to add`|
|`source_ref`|`STRING`|`TODO: Here the relevant description pending to add`|
                    
#### Example
```sql 
                CALL txflow.function_group.function_name( 
                    'project.dataset.input_table',
 
                    'project.dataset.output_table', 
                    ['rowhash'], 
                    ['timestamp
 DESC', 'additional_sort_column ASC'] 
                ) 
```                    
--------------------------------


## Function group: profile | Function name: get_column_profile
Description of the function group: profile and function name: get_column_profile

### `txflow.profile.get_column_profile`
                    
|function_group|function_name|function_output|description|
| :--- | :--- | :--- | :--- |
|`profile`|`get_column_profile`|e.g. VIEW|Get column profiles for each column in a single table or view|

#### Call Syntax
```sql 
 CALL txflow.profile.get_column_profile( 
        'sql_block_name__STRING'
        
'sql_blocks__ARRAY<STRUCT<name STRING, code STRING, dependencies ARRAY<STRING>>>'
        'source_ref__STRING'
)
```                    
#### Arguments

|argument|datatype|description|
| :--- | :--- | :--- |
|`sql_block_name`|`STRING`|`TODO: Here the relevant description pending to add`|
|`sql_blocks`|`ARRAY<STRUCT<name STRING, code STRING, dependencies ARRAY<STRING>>>`|`TODO: Here the relevant description pending to add`|
|`source_ref`|`STRING`|`TODO: Here the relevant description pending to add`|
                    
#### Example
```sql 
                CALL txflow.function_group.function_name( 
                    'project.dataset.input_table',
 
                    'project.dataset.output_table', 
                    ['rowhash'], 
                    ['timestamp
 DESC', 'additional_sort_column ASC'] 
                ) 
```                    
--------------------------------
