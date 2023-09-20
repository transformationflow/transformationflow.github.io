These functions are used to parse and manipulate `ARRAY` inputs.

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

## **`array_remove_empty_elements`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `array_remove_empty_elements`
_**ID**_ | `bqtools.[region].array_remove_empty_elements`
_**Description**_ | Remove empty string elements in an `ARRAY`
_**Type**_ | `FUNCTION`
_**Arguments**_ | `input_array ARRAY<STRING>`
_**Returns**_ | `ARRAY<STRING>`
_**Dependencies**_ | `None`

!!! info "execution: `array_remove_empty_elements`"
    === "EU"
        ```sql
        SELECT bqtools.eu.array_remove_empty_elements(input_array);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.array_remove_empty_elements(input_array);
        ```

## **`array_exclude_elements`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `array_exclude_elements`
_**ID**_ | `bqtools.[region].array_exclude_elements`
_**Description**_ | Filter through the `input_array` and returns a new `ARRAY<STRING>` with elements that are not in the `exclude_array` `ARRAY`.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `input_array ARRAY<STRING>, exclude_array ARRAY<STRING>`
_**Returns**_ | `ARRAY<STRING>`
_**Dependencies**_ | `None`

!!! info "execution: `array_exclude_elements`"
    === "EU"
        ```sql
        SELECT bqtools.eu.array_exclude_elements(input_array, exclude_array);
        ```

    === "US"
        ```sql
        SELECT bqtools.us.array_exclude_elements(input_array, exclude_array);
        ```