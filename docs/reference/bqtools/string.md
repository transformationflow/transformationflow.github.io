These functions are used to parse and manipulate `STRING` inputs.

# Functions
## **`parse_resource_id`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `parse_resource_id`
_**ID**_ | `bqtools.[region].parse_resource_id`
_**Description**_ | Parses valid `resource_ids` into constituent elements.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `resource_id STRING`
_**Returns**_ | `STRUCT(is_valid BOOL, project_id STRING, dataset_name STRING, resource_name STRING, dataset_id STRING, resource_id STRING`
_**Dependencies**_ | `None`

!!! info "execution: `parse_resource_id`"
    === "EU"
        ```sql
        SELECT bqtools.eu.parse_resource_id(resource_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.parse_resource_id(resource_id);
        ```

## **`encode_uri_component`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `encode_uri_component`
_**ID**_ | `bqtools.[region].encode_uri_component`
_**Description**_ | Encodes a [URI](https://developer.mozilla.org/en-US/docs/Glossary/URI) (Uniform Resource Identifier) by replacing each instance of certain characters by one, two, three, or four escape sequences representing the [UTF-8](https://developer.mozilla.org/en-US/docs/Glossary/UTF-8) encoding of the character. Functional SQL implementation of the JavaScript function [encodeURIComponent](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent).
_**Type**_ | `FUNCTION (JS)`
_**Arguments**_ | `input_uri STRING`
_**Returns**_ | `STRING`
_**Dependencies**_ | `encodeURIComponent()`

!!! info "execution: `encode_uri_component`"
    === "EU"
        ```sql
        SELECT bqtools.eu.decode_uri_component(input_uri);
        ```

    === "US"
        ```sql
        CALL bqtools.us.decode_uri_component(input_uri);
        ```

??? note "example: `encode_uri_component`"

    ```sql
    SELECT bqtools.eu.decode_uri_component("test?");
    ```

    ```sql
    response: "test%3F"
    ```



## **`decode_uri_component`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `decode_uri_component`
_**ID**_ | `bqtools.[region].decode_uri_component`
_**Description**_ | Decodes a [URI](https://developer.mozilla.org/en-US/docs/Glossary/URI) (Uniform Resource Identifier) component.  Functional SQL implementation of the JavaScript function [decodeURIComponent](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent).
_**Type**_ | `FUNCTION (JS)`
_**Arguments**_ | `input_uri STRING`
_**Returns**_ | `STRING`
_**Dependencies**_ | `decodeURIComponent()`

!!! info "execution: `decode_uri_component`"
    === "EU"
        ```sql
        SELECT bqtools.eu.decode_uri_component(input_uri);
        ```

    === "US"
        ```sql
        CALL bqtools.us.decode_uri_component(input_uri);
        ```

??? note "example: `decode_uri_component`"

    ```sql
    SELECT bqtools.eu.decode_uri_component("test%3F");
    ```

    ```sql
    response: "test?"
    ```