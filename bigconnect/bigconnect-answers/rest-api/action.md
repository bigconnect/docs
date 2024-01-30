# Action

`/api/action/` endpoints.

### `DELETE /api/action/:action-id`

PARAMS:

* **`action-id`** value must be an integer greater than zero.

### `DELETE /api/action/:id/public_link`

Delete the publicly-accessible link to this Dashboard.

PARAMS:

* **`id`** value must be an integer greater than zero.

### `GET /api/action/`

Returns actions that can be used for QueryActions. By default lists all viewable actions. Pass optional `?model-id=<model-id>` to limit to actions on a particular model.

PARAMS:

* **`model-id`** nullable value must be an integer greater than zero.

### `GET /api/action/:action-id`

PARAMS:

* **`action-id`** value must be an integer greater than zero.

### `GET /api/action/:action-id/execute`

Fetches the values for filling in execution parameters. Pass PK parameters and values to select.

PARAMS:

* **`action-id`** value must be an integer greater than zero.
* **`parameters`** value must be a valid JSON string.

### `GET /api/action/public`

Fetch a list of Actions with public UUIDs. These actions are publicly-accessible _if_ public sharing is enabled.

### `POST /api/action/`

Create a new action.

PARAMS:

* **`visualization_settings`** nullable map
* **`parameters`** nullable sequence of map
* **`description`** nullable string
* **`error_handle`** nullable string, and must be a valid json-query, something like ‘.item.title’
* **`database_id`** nullable value must be an integer greater than zero.
* **`name`** string
* **`response_handle`** nullable string, and must be a valid json-query, something like ‘.item.title’
* **`template`** nullable map where {:method -> \<enum of GET, POST, PUT, DELETE, PATCH>, :url -> = 1>, :body (optional) -> , :headers (optional) -> , :parameters (optional) -> , :parameter\_mappings (optional) -> } with no other keys
* **`type`** nullable Unsupported action type
* **`dataset_query`** nullable map
* **`model_id`** value must be an integer greater than zero.
* **`kind`** nullable Unsupported implicit action kind
* **`parameter_mappings`** nullable map
* **`action`**

### `POST /api/action/:id/execute`

Execute the Action.

`parameters` should be the mapped dashboard parameters with values.

PARAMS:

* **`id`** value must be an integer greater than zero.
* **`parameters`** nullable map from to
* **`_body`**

### `POST /api/action/:id/public_link`

Generate publicly-accessible links for this Action. Returns UUID to be used in public links. (If this Action has already been shared, it will return the existing public link rather than creating a new one.) Public sharing must be enabled.

You must be a superuser to do this.

PARAMS:

* **`id`** value must be an integer greater than zero.

### `PUT /api/action/:id`

PARAMS:

* **`id`** value must be an integer greater than zero.
* **`action`** map where {:archived (optional) -> , :database\_id (optional) -> , :dataset\_query (optional) -> , :description (optional) -> , :error\_handle (optional) -> \<nullable string, and must be a valid json-query, something like '.item.title'>, :kind (optional) -> , :model\_id (optional) -> , :name (optional) -> , :parameter\_mappings (optional) -> , :parameters (optional) -> , :response\_handle (optional) -> \<nullable string, and must be a valid json-query, something like '.item.title'>, :template (optional) -> \<nullable map where {:method -> \<enum of GET, POST, PUT, DELETE, PATCH>, :url -> = 1>, :body (optional) -> , :headers (optional) -> , :parameters (optional) -> , :parameter\_mappings (optional) -> } with no other keys>, :type (optional) -> , :visualization\_settings (optional) -> }
