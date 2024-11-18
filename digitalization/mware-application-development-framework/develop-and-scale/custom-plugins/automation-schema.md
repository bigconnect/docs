# Automation schema

The automation schema is one of the critical elements that is required for developing a custom automation action. The first step to creating your plugin is defining everything that your automation action will require/return. Below is an example of a basic automation schema - this is the same schema that is provided if you initialize a new automation plugin from the CLI.

Automation schema.json

```Text
{
  "type": "automation",
  "metadata": {},
  "schema": {
    "name": "automation",
    "tagline": "An amazing automation!",
    "icon": "Actions",
    "description": "An amazing automation!",
    "type": "action",
    "stepId": "automation",
    "inputs": {
      "text": ""
    },
    "schema": {
      "inputs": {
        "properties": {
          "text": {
            "type": "string",
            "title": "Log"
          }
        },
        "required": ["text"]
      },
      "outputs": {
        "properties": {
          "success": {
            "type": "boolean",
            "description": "Whether the action was successful"
          },
          "message": {
            "type": "string",
            "description": "What was output"
          }
        },
        "required": ["success", "message"]
      }
    }
  }
}

```

#### Properties

The following section walks through all of the automation properties, covering their usage and types.

**Top level properties**

Documents the top level properties that all automation plugin schemas should contain.

| Property | Type   | Description                                                                                         |
| -------- | ------ | --------------------------------------------------------------------------------------------------- |
| type     | string | The type of plugin, for an automation this should always be `"automation"`.                         |
| metadata | object | A metadata structure, used to document information about the plugin that isn't used by the builder. |
| schema   | object | The main schema, defining the inputs and outputs of the automation action.                          |

**Schema structure**

The main schema of the automation action, which defines the core elements of the action.

| Property    | Type   | Description                                                                                                                                                                                                                                         |
| ----------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name        | string | The name of the automation action - this will be displayed in the builder when looking through actions to add/displaying an automation using the plugin.                                                                                            |
| tagline     | string | A short description which is displayed when showing the full automation flow in the builder.                                                                                                                                                        |
| icon        | string | A Spectrum Icon to represent your automation action which will be used in the builder - this is case sensitive, for example if you wanted to use the "Journey Action" icon then you would set `JourneyAction` in the schema.                        |
| description | string | A longer description, not currently used in builder.                                                                                                                                                                                                |
| type        | string | The type of automation step, the only valid choices for an action are `action` or `logic` - for most plugins `action` will be the correct choice.                                                                                                   |
| stepId      | string | a unique identifier which will be used to link your automation action to automations that use it. This can be anything, for example a UUID is fine, as long as no other automation action plugin uses the same step ID in your system it will work. |
| inputs      | object | A map of different input options for your automation - this is not strictly required, but can be used to set defaults for different input properties.                                                                                               |
| schema      | object | The main inputs and outputs that your automation action will utilize.                                                                                                                                                                               |

**Input/output structure**

The `schema` property of the automation defines the various inputs and outputs of the action. This is one of the most key elements, with the structure for `inputs` and `outputs` being identical, as shown below.

| Property   | Type      | Description                                                                                                                             |
| ---------- | --------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| properties | object    | A map of the different properties that are taken/returned (depending on whether it is the `inputs` or `outputs` section of the schema). |
| required   | string\[] | A array of the different properties which are required - the builder will throw an error if an action input is not filled in.           |

The properties structure is the most important one in this case, for example in the schema example shown about, in the input properties we have `text` - defining the input text which should be logged by the action. Each listed property can contain the following elements:

| Property    | Type      | Description                                                                                                                                                                                                                                           |
| ----------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type        | string    | The type of input/output, these are mainly primitive types, the options are `object`, `string`, `boolean`, `number` and `array`. It should be noted that `object` and `array` cannot be used for inputs.                                              |
| customType  | string    | There are a few custom types which can be used to get specify that an input/output is something more specific - the builder will change how it displays the input based on this, or provide more information in the binding context menu for outputs. |
| title       | string    | How the input/output is displayed in the builder, for example if the property is `some_input` then a title can be used to present a human readable label on the input/output.                                                                         |
| description | string    | A description of the input/output, this is mainly used for outputs as it can be used to provide context in the bindings menu.                                                                                                                         |
| enum        | string\[] | If a dropdown of options is required then this can be used to provide a list which the builder will display, instead of expecting a free-text input from the user. This is only used for inputs.                                                      |
| pretty      | string\[] | If the options (`enum`) are not human-readable, then a separate list of easily readable strings can be provided which will be used as labels in the dropdown. This is only used for inputs.                                                           |

**Custom types**

The custom type can be used to provide some additional context - these can be a little bit more difficult to use, for context of how they are used with the built-in automation actions you can look through their definitions for inspiration.

| Type          | Base type | Description                                                                                                                                                                                                                                                            |
| ------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `table`       | `string`  | This will display a list of tables which the user can select from - the automation will be provided with the table ID they have selected.                                                                                                                              |
| `row`         | `object`  | A single row, used for outputs if the action will return a single row.                                                                                                                                                                                                 |
| `rows`        | `array`   | A list of rows, used for outputs, if the action will return a number of rows.                                                                                                                                                                                          |
| `wide`        | `string`  | A wide text input - used for more complex text input.                                                                                                                                                                                                                  |
| `query`       | `string`  | This will display a list of queries that have been created against datasources within the user's app - the automation action will receive the query ID which it can use to trigger the query via the [API](https://docs.budibase.com/reference/post\_queries-queryid). |
| `queryParams` | `string`  | Used in conjunction with the `"query"` custom type - requesting the user fill in all of the required inputs of the selected query.                                                                                                                                     |
| `queryLimit`  | `number`  | Used in conjunction with the `"filters"` custom type, this specifies a number of rows to be returned.                                                                                                                                                                  |
| `code`        | `string`  | Prompts the user for a code block, this could be any sort of code, but by default a code mirror editor will be shown configured for JavaScript.                                                                                                                        |
| `filters`     | `object`  | Generates a filter structure, like is used by the rows API for searching - as described in the [API](https://docs.budibase.com/reference/post\_tables-tableid-rows-search).                                                                                            |
| `column`      | `string`  | Allows picking a specific column name from a selected table, this requires a property with a custom type of `"table"` to work.                                                                                                                                         |
