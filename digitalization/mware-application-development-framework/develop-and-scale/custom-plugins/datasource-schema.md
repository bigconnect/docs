# Datasource schema

The datasource schema is one of the critical elements that is required for developing a custom datasource. The first step to creating your plugin is defining everything that your datasource will require/return. Below is an example of a basic datasource schema - this is the same schema that is provided if you initialise a new datasource plugin from the CLI.

Datasource schema.json

```Text
{
  "type": "datasource",
  "metadata": {},
  "schema": {
    "docs": "https://docs.com",
    "friendlyName": "Basic HTTP",
    "type": "API",
    "description": "Performs a basic HTTP calls to a URL",
    "datasource": {
      "url": {
        "type": "string",
        "required": true
      },
      "cookie": {
        "type": "string",
        "required": false
      }
    },
    "query": {
      "create": {
        "type": "json"
      },
      "read": {
        "type": "fields",
        "fields": {
          "queryString": {
            "display": "Query string",
            "type": "string",
            "required": false
          }
        }
      },
      "update": {
        "type": "json"
      },
      "delete": {
        "type": "fields",
        "fields": {
          "id": {
            "type": "string",
            "required": true
          }
        }
      }
    }
  }
}
```

#### Properties

The following section walks through all of the datasource properties, covering their usage and types.

**Top level properties**

Documents the top level properties that all datasource plugin schemas should contain.

| Property                                                                           | Type   | Description                                                                                                   |
| ---------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------- |
| type                                                                               | string | The type of plugin, for a datasource this should always be `"datasource"`.                                    |
| metadata                                                                           | object | A metadata structure, used to document information about the plugin that isn't used by the builder.           |
| [schema](https://docs.budibase.com/docs/custom-datasource-schema#schema-structure) | object | The main schema, defining the structures required for the datasource and its queries, as well as its outputs. |

**Schema structure**

The main schema of the datasource, used to define three core elements of your datasource and its queries:

1. The datasource inputs, used to create the overall datasource
2. The query types, by default this will be the primary CRUD operations (create, read, update delete) but custom ones can be added
3. The inputs required by each query type

Below the properties of the schema have been defined.

| Property                                                                                | Type   | Description                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| docs                                                                                    | string | A URL/link to the documentation for the datasource, for example for a database you could provide a link to the database API.                                                                                                                |
| friendlyName                                                                            | string | A human readable name which is used instead of the plugin name. This is useful as your plugin name should not be defined with special characters, spaces etc. When creating a datasource this is the name that will be seen in the builder. |
| type                                                                                    | string | Defines the type of datasource, this is displayed along with the friendlyName when creating datasources in the builder. For example we might define a database as `"Non-relational"` or `"Relational"`.                                     |
| description                                                                             | string | A description of the datasource, used when creating the datasource in the builder.                                                                                                                                                          |
| [datasource](https://docs.budibase.com/docs/custom-datasource-schema#datasource-schema) | object | The datasource schema, defining the properties used to create the datasource. These will be available to every query and are supplied to the constructor of your datasource.                                                                |
| [query](https://docs.budibase.com/docs/custom-datasource-schema#query-schema)           | object | The query schema, defining the properties used to execute a query against your datasource.                                                                                                                                                  |

**Datasource schema**

This object documents the various properties that are required to create the datasource. An example of properties that would be used here is database connection details, a URL to query; generally properties which are common across all queries. Each key within this structure defines a different property, like such:

Datasource JSON

```Text
"datasource": {
  "<property 1>": {
    <structure>
  },
  "<property 2>": {
    <structure> 
  }
}
```

The structure of each of these keys is as follows:

| Property | Type                                                                                        | Description                                                                                        |
| -------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| type     | [DatasourceFieldType](https://docs.budibase.com/docs/custom-datasource-schema#enumerations) | The type of the input.                                                                             |
| required | boolean                                                                                     | Whether the property is required - the datasource cannot be created within defining this property. |

**Query schema**

Each property of the query object defines a different query type, e.g. `create`, `read`, `update` or `delete`. The properties of each of these keys is defined below.

| Property | Type                                                                              | Description                                                                                                                        |
| -------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| type     | [QueryType](https://docs.budibase.com/docs/custom-datasource-schema#enumerations) | The type of the query property.                                                                                                    |
| fields   | object                                                                            | If the query type has been set to `"fields"` then a fields structure must be defined, to state what properties should be expected. |

**Fields schema**

When a query takes a fields input type it must specify what fields it expects, and what types to expect. Each field is specified as a key value structure as follows:

Fields JSON

```Text
"fields": {
  "<field 1>": {
    <structure>
  },
  "<field 2>": {
    <structure>
  }
}
```

The structure of each of the fields is as follows:

| Property | Type                                                                                        | Description                                                                                                                                                  |
| -------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| display  | string                                                                                      | An optional property to define what the field should be displayed as in the builder. This is useful incase the property does not have a human readable name. |
| type     | [DatasourceFieldType](https://docs.budibase.com/docs/custom-datasource-schema#enumerations) | The type of the field, this defines what type of input will be used in the builder.                                                                          |
| required | boolean                                                                                     | Whether the field is required to carry out the query.                                                                                                        |

**Enumerations**

A general table of the enumerated values that can be used throughout the datasource schema. Each enumeration value is comma separated and case sensitive. It should be noted that these enumerations can be found in the types package.

| Name                | Values                                                                                                    |
| ------------------- | --------------------------------------------------------------------------------------------------------- |
| DatasourceFieldType | `"string"`, `"longForm"`, `"boolean"`, `"number"`, `"password"`, `"list"`, `"object"`, `"json"`, `"file"` |
| QueryType           | `"sql"`, `"json"`, `"fields"`                                                                             |
