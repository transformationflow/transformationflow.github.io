These functions are used to parse and manipulate `DATE` inputs.

# **`convert_date_to_date_id`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `convert_date_to_date_id`
_**ID**_ | `bqtools.[region].convert_date_to_date_id`
_**Description**_ | Converts a `DATE` value to a `date_id` `STRING`.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `date DATE`
_**Returns**_ | `STRING`
_**Dependencies**_ | `None`

!!! info "execution: `convert_date_to_date_id`"
    === "EU"
        ```sql
        SELECT bqtools.eu.convert_date_to_date_id(date);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.convert_date_to_date_id(date);
        ```

# **`convert_date_id_to_date`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `convert_date_id_to_date`
_**ID**_ | `bqtools.[region].convert_date_id_to_date`
_**Description**_ | Converts a `date_id` `STRING` `DATE` value to a .
_**Type**_ | `FUNCTION`
_**Arguments**_ | `date DATE`
_**Returns**_ | `STRING`
_**Dependencies**_ | `None`

!!! info "execution: `convert_date_id_to_date`"
    === "EU"
        ```sql
        SELECT bqtools.eu.convert_date_id_to_date(date);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.convert_date_id_to_date(date);
        ```

# **`generate_date_id_array`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `generate_date_id_array`
_**ID**_ | `bqtools.[region].generate_date_id_array`
_**Description**_ | Builds an array of `date_ids` between a `start_date` and `end_date`.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `start_date DATE, end_date DATE`
_**Returns**_ | `ARRAY<STRING>`
_**Dependencies**_ | `None`

!!! info "execution: `generate_date_id_array`"
    === "EU"
        ```sql
        SELECT bqtools.eu.generate_date_id_array(start_date, end_date);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.generate_date_id_array(start_date, end_date);
        ```