These functions are used to parse and manipulate `STRING` inputs.

# Functions
## **`parse_resource_id`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `parse_resource_id`
_**ID**_ | `bqtools.[region].parse_resource_id`
_**Description**_ | Parses valid `resource_ids` into constituent components.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `resource_id STRING`
_**Returns**_ | `STRUCT<is_valid BOOL, project_id STRING, dataset_name STRING, resource_name STRING, dataset_id STRING, resource_id STRING>`
_**Dependencies**_ | `None`

!!! info "execution: `parse_resource_id`"
    === "EU"
        ```sql
        SELECT bqtools.eu.parse_resource_id(resource_id);
        ```
        Individual components can be directly accessed using the following syntax (this will return the `resource_name`):
        ```sql
        SELECT bqtools.eu.parse_resource_id(resource_id).resource_name;
        ```


    === "US"
        ```sql
        SELECT bqtools.us.parse_resource_id(resource_id);
        ```
        Individual components can be directly accessed using the following syntax (this will return the `resource_name`):
        ```sql
        SELECT bqtools.us.parse_resource_id(resource_id).resource_name;
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

## **`replace_resource_project_id`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `replace_resource_project_id`
_**ID**_ | `bqtools.[region].replace_resource_project_id`
_**Description**_ | Replaces the `resource_project_id` of the `original_resource_id` by the new `replacement_project_id`.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `original_resource_id STRING, replacement_project_id STRING`
_**Returns**_ | `resource_id STRING`
_**Dependencies**_ | `bqtools.[region].parse_resource_id`

!!! info "execution: `replace_resource_project_id`"
    === "EU"
        ```sql
        SELECT bqtools.eu.replace_resource_project_id(original_resource_id, replacement_project_id);
        ```

    === "US"
        ```sql
        CALL bqtools.us.replace_resource_project_id(original_resource_id, replacement_project_id);
        ```

??? note "example: `replace_resource_project_id`"

    ```sql
    DECLARE original_resource_id STRING;
    DECLARE replacement_project_id STRING;

    SET original_resource_id = "project_id.dataset_name.resource_name";
    SET replacement_project_id = "new_project_id";

    SELECT bqtools.eu.replace_resource_project_id(original_resource_id, replacement_project_id);
    ```

    ```sql
    response: "new_project_id.dataset_name.resource_name"
    ```

## **`replace_resource_dataset_name`**
_**Attribute**_ | Value
--- | ---
_**Name**_ | `replace_resource_dataset_name`
_**ID**_ | `bqtools.[region].replace_resource_dataset_name`
_**Description**_ | Replaces the `resource_dataset_name` of the `original_resource_id` by the new `replacement_dataset_name`.
_**Type**_ | `FUNCTION`
_**Arguments**_ | `original_resource_id STRING, replacement_dataset_name STRING`
_**Returns**_ | `resource_id STRING`
_**Dependencies**_ | `bqtools.[region].parse_resource_id`

!!! info "execution: `replace_resource_dataset_name`"
    === "EU"
        ```sql
        SELECT bqtools.eu.replace_resource_dataset_name(original_resource_id, replacement_dataset_name);
        ```

    === "US"
        ```sql
        CALL bqtools.us.replace_resource_dataset_name(original_resource_id, replacement_dataset_name);
        ```

??? note "example: `replace_resource_project_id`"

    ```sql
    DECLARE original_resource_id STRING;
    DECLARE replacement_dataset_name STRING;

    SET original_resource_id = "project_id.dataset_name.resource_name";
    SET replacement_dataset_name = "new_dataset_name";

    SELECT bqtools.eu.replace_resource_dataset_name(original_resource_id, replacement_dataset_name);
    ```

    ```sql
    response: "project_id.new_dataset_name.resource_name"
    ```
