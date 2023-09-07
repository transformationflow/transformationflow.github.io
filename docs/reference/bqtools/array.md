These functions are used to parse and manipulate and `ARRAY` inputs.

# Functions
## **`array_unique_string`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `array_unique_string`
_**ID**_ | `bqtools.[region].array_unique_string`
_**Description**_ | Performs a `DISTINCT` operation on `STRING` elements in an `ARRAY`
_**Type**_ | `FUNCTION`
_**Arguments**_ | `input_array ARRAY<STRING>`
_**Returns**_ | `ARRAY<STRING>`
_**Dependencies**_ | `None`

!!! info "execution: `array_unique_string`"
    === "EU"
        ```sql
        SELECT bqtools.eu.array_unique_string(input_array);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.array_unique_string(input_array);
        ```