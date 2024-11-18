# Component schema

Provides a number of attributes that determine the available options in the settings panel and how the component is displayed in the component tree.

Required fields are marked by an asterisk.

| Property                                                               | Type    | Description                                                                                                                                                                                                                                                              |
| ---------------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name\*                                                                 | string  | A unique name for your component.                                                                                                                                                                                                                                        |
| friendlyName\*                                                         | string  | The name that is displayed in the app builder.                                                                                                                                                                                                                           |
| icon\*                                                                 | string  | <p>The name of the icon that is shown in the component tree.<br><br><em>A full list of the available icons can be found</em> <a href="https://spectrum.adobe.com/page/icons/"><em>here</em></a><em>. Simply remove any spaces from the names, e.g. DeviceLaptop</em></p> |
| [settings\*](https://docs.budibase.com/docs/component-schema#settings) | array   | The list of additional settings beyond the defaults.                                                                                                                                                                                                                     |
| [_context_](https://docs.budibase.com/docs/custom-component#context)   | array   | A list of data bindings that are provided by the component.                                                                                                                                                                                                              |
| _styles_                                                               | array   | <p>Shorthand for adding settings for styling.<br><br>Includes: <code>"margin", "padding", "size", "background", "border"</code></p>                                                                                                                                      |
| _hasChildren_                                                          | boolean | Determines if the component can be a container for other components or not.                                                                                                                                                                                              |
| _showSettingsBar_                                                      | boolean | Determines if the preview settings bar will float above a selected component.                                                                                                                                                                                            |
| _info_                                                                 | string  | An optional info message to display at the bottom of the component settings in the builder.                                                                                                                                                                              |

#### Settings

The `settings` array defines which settings are rendered inside the builder for this component. If a settings object contains `"section": true` then it is expected to be a section with its own array of settings.

Required fields are marked by an asterisk.

| Property                                                                      | Type                | Description                                                                                                                                                                                                                                                                                                |
| ----------------------------------------------------------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| type\*                                                                        | string              | <p>The type of the setting.<br><br><em>A full list of setting types can be found in the componentMap</em></p>                                                                                                                                                                                              |
| key\*                                                                         | string              | A unique identifier for the setting                                                                                                                                                                                                                                                                        |
| label\*                                                                       | string              | The label to show in the builder for this setting                                                                                                                                                                                                                                                          |
| _dependsOn_                                                                   | string \| DependsOn | <p>Specifies another setting which this one depends on. This setting will not be shown unless the other setting exists.<br><br><em>If a string value is provided, a single setting key is expected. If an object, setting and value keys are expected, which will check against a specific value.</em></p> |
| _defaultValue_                                                                | any                 | The default value for this setting for newly created components                                                                                                                                                                                                                                            |
| [_options_](https://docs.budibase.com/docs/component-schema#settings-options) | array               | A list of options for settings of type `select`                                                                                                                                                                                                                                                            |
| _min_                                                                         | number              | The minimum value of the setting                                                                                                                                                                                                                                                                           |
| _max_                                                                         | number              | The maximum value of the setting                                                                                                                                                                                                                                                                           |
| _showInBar_                                                                   | boolean             | Whether to show this setting in the preview settings bar, if enabled for this component                                                                                                                                                                                                                    |
| _barStyle_                                                                    | BarStyle            | The style of bar component to show for this setting                                                                                                                                                                                                                                                        |
| _placeholder_                                                                 | string              | The placeholder value to be passed to the rendered settings component                                                                                                                                                                                                                                      |
| _section_                                                                     | boolean             | <p>Indicates if a settings section is being defined<br><br><em>Sections cannot be nested within another section</em></p>                                                                                                                                                                                   |
| <p><em>name</em><br>[section only]</p>                                        | string              | The name of the section                                                                                                                                                                                                                                                                                    |

**Depends on**

If a string value is provided, then the setting will only show if the specified field is present, i.e. not _null_, _undefined_, _empty_, _false_ or 0.

Alternatively an object can be provided with the following properties. Required fields marked by an asterisk.

| Key       | Type    | Description                                                            |
| --------- | ------- | ---------------------------------------------------------------------- |
| setting\* | string  | The name of the setting to depend on                                   |
| value\*   | any     | The value the 'depends on' setting must be for this setting to present |
| _invert_  | boolean | If true, the 'depends on' setting must NOT match the specified _value_ |

**Select options**

If a setting is of type `select`, then an options array must also be specified to control which options are available for the setting. The options passed in must have the following structure:

| Key      | Type   | Description                                                                                                                                                                                                                                                                                                                                           |
| -------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| label    | string | The readable label shown for this option                                                                                                                                                                                                                                                                                                              |
| value    | any    | The option value                                                                                                                                                                                                                                                                                                                                      |
| barIcon  | string | <p>If showing this setting in the settings bar and using a bar type of buttons, this key defines which spectrum icon to render in the bar<br><br><em>A full list of the available icons can be found</em> <a href="https://spectrum.adobe.com/page/icons/"><em>here</em></a><em>. Simply remove any spaces from the names, e.g. DeviceLaptop</em></p> |
| barTitle | string | If showing this setting in the settings bar and using a bar type of buttons, this key defines the title text to be shown when hovering over the icon                                                                                                                                                                                                  |

**Enumerations**

A general table of the enumerated values that can be used throughout the component schema. Each enumeration value is comma separated and case sensitive.

| Name     | Values              |
| -------- | ------------------- |
| BarStyle | `buttons`, `picker` |
