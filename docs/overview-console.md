# Transformation Flow Console
The transformation flow console is the final component, which gives the SQL developer a powerful user interface to build transformation flows, with data profiling and documentation in place.  The Transformation Flow Console is currently in private alpha.

## Authentication
In order to access your data in BigQuery and the `txflow` functions, you will need to upload your credentials in the form of a JSON key file.  These credentials are not stored and the key file contents are only used within the user session to authenticate. Once uploaded, click the 'AUTHENTICATE' button.  You will see a message upon successful authentication indicating the project(s) to which you are authenticated.

## Flow Builder Inputs
Once authentication is sucessful, you can minimise the sidebar and focus on the main flow builder. 

### Flow Stage Name
The first step is to add a name for the flow stage.  This should be in snake case (e.g. `inbound_data`) and unique within the transformation flow.  This will be the `sql_block_name` in `sql_blocks` and if building any views from the transformation flow, it will be the name of the Common Table Expression.  Try to make this descriptive to the particular stage as this will make the final code more readable (e.g. `filter_out_null_accounts`).  Note that non-compliant flow stage names will be corrected to the required format.

### Query vs. Function Input
The next important step is to determine whether the input will be a hand-crafted query or a function.  

#### Query Input
If writing a query, then leave the `Input Type` radio button set to `query` and simply type or paste the query into the Code Editor and APPLY (CMD+ENTER) when complete.

#### Function Input
If the flow stage is a function, then select `function` from the `Input Type` radio button and a dropdown will appear with the full list of available `txflow` functions.  You can select from the dropdown or begin typing and the options will filter based on your input. Selecting a function will display the documentation in the UI as well as presenting input fields for the specific arguments required for the function.

Once the function has been selected the user needs to click on the [] button which will then populate the Code Editor with the function call code and additional 

### Generating SQL

## Flow Stage Profiling


## Flow Builder Outputs


## Performance
Data vs. SQL





