# Text inputs

In Mware Application Development Framework several fields require user key-press input. They are:

* JSON field
* Long form field
* Number field
* Password field
* Text field
* BigInt field

### Adding text inputs

In all cases these components must be nested within a Form component.

Within the component panel of your screen click `Add component` and select the Form component.

Next click on the form component that was just added, then click add component again. This will ensure that the field will be nested inside the form once added, however you can also drag orphan input fields into your form if needed.

When looking for a key-press input, a handy tip is to search for the word 'field'.

<figure><img src="https://files.readme.io/1e2467000960cda49c1c5e181ab7c1c293b3b0f01738527d1c41307d7d57a3a7-Screenshot_2024-10-01_at_08.31.40.png" alt="Search for and select a field to add" width="375"><figcaption><p>Search for and select a field to add</p></figcaption></figure>

<figure><img src="https://files.readme.io/14471d3c29673cc558f2c3ba4966bd80958ab20ea02ce2c6ff0895a0212fd9d1-Screenshot_2024-10-01_at_08.36.26.png" alt="Example form component tree" width="375"><figcaption><p>Example form component tree</p></figcaption></figure>

Finally, for an input to be useable, it must be given a field name. If the form has been given a [schema](https://docs.budibase.com/docs/forms#form-schema), you will be able to select from a list of type-matched fields. Alternatively, you can manually enter a field name.

<figure><img src="https://files.readme.io/e73b41badc69f18554df08f495dc2e28af1e01d8a2edc6e69386108d0ffbe551-Screenshot_2024-10-01_at_08.41.05.png" alt="Selecting a field from the form schema" width="375"><figcaption><p>Selecting a field from the form schema</p></figcaption></figure>

<figure><img src="https://files.readme.io/16da35a22935dc342ff58607a60ec1b2e14f172905f53020b09f9ba30b2b60a6-Screenshot_2024-10-01_at_08.34.19.png" alt="Fields are now visible"><figcaption><p>Fields are now visible</p></figcaption></figure>

### Using form blocks

When using a Form block the field components are not directly accessible. If you want to edit the field components, then you will need to Eject the form block.

Otherwise, Mware Application Development Framework will match the Data type of the field to a corresponding field component. Most commonly this will be the **Text field** and **Number field**, however it could also be the **JSON field** for the JSON data type, among others.

### Common settings

All of the key-press inputs share common settings, which are presented in the reference table below.

| Setting       | Description                                                                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Field         | Connected field from the parent Form element, or custom by typing in a field name.                                                                     |
| Label         | The text visible to the user next to the input box as a descriptor of the field.                                                                       |
| Placeholder   | The text displayed in a lighter tint inside the input box. Will only display when nothing is entered in the field.                                     |
| Default Value | On initial load of the form the field will be pre-filled with specified default value. Entering this will also prevent the `placeholder` from showing. |
| Disabled      | When checked, disables the field; the user cannot change the value.                                                                                    |

#### On change

Allows you to specify [Actions](https://docs.budibase.com/docs/actions) to be triggered when the input field changes value. Specifically this event is triggered when the user presses the tab key or clicks off the field (on blur), or if the user pressed the enter key.

When using the field value as a Binding, make sure to use the current field value. For example, you may want to use the first name value in an on change action:

<figure><img src="https://files.readme.io/15a969f-Screenshot_2022-12-13_at_11.37.18.png" alt="Select the field that triggered the on change"><figcaption><p>Select the field that triggered the on change</p></figcaption></figure>

<figure><img src="https://files.readme.io/c72ca4d-Screenshot_2022-12-13_at_11.37.28.png" alt="Select Field Value; i.e. the on change value"><figcaption><p>Select Field Value; i.e. the on change value</p></figcaption></figure>

> ### 👍`{{ Field Value }}`
>
> Gets the current field value.

> ### ❗️`{{ Form.Fields.First Name }}`
>
> Will get the previous value; before the on change was triggered.

#### Configure validation

Allows you to configure custom validation rules for the input field.\
See more about [Form validation](https://docs.budibase.com/docs/form-validation).

### Text field

The text field allows users to enter inline text. This is the most common text input.

It is compatible with the [Text](https://docs.budibase.com/docs/text) data type.

![836](https://files.readme.io/1b3802d-textfield.png)

### Number field

Allows users to enter whole or decimal numbers.

It is compatible with the [Number](https://docs.budibase.com/docs/number) data type.

![](https://files.readme.io/8fb7f67-number-field.png)

### Password field

Same as the text field, however it hides the text being entered.

Any binding of the field value will be in plain text, and Mware Application Development Framework  does not perform any hashing by default on password fields when saving rows.

![](https://files.readme.io/6e13376-password-field.png)

### Long form field

The long form field is a text field that allows for longer and multiline input.

![](https://files.readme.io/65fabc6-long-form-field.png)

You can also enable rich-text editing with markdown support by selecting `Rich text` in the formatting setting.

<figure><img src="https://files.readme.io/a678858-Screenshot_2022-12-13_at_12.51.32.png" alt="Selecting rich text formatting"><figcaption><p>Selecting rich text formatting</p></figcaption></figure>

![](https://files.readme.io/f3b6ce5-long-form-markdown.png)

Usually when [importing a table from CSV](https://docs.budibase.com/docs/budibasedb#csv-upload) or [fetching SQL tables](https://docs.budibase.com/docs/sql-datasource#fetching-tables) string data types will automatically be assigned as a Text type.

If you want to support the long form field, then you will need to [edit column](https://docs.budibase.com/docs/budibasedb#editing-columns) and change the type.

<figure><img src="https://files.readme.io/6274658-Screenshot_2022-12-13_at_13.00.42.png" alt="Allowing the address to be multi-line"><figcaption><p>Allowing the address to be multi-line</p></figcaption></figure>

### JSON field

The JSON field allows you to enter [JSON](https://docs.budibase.com/docs/json) into resizable box with formatting enabled.

![](https://files.readme.io/8fec4ee-json-box\_Medium.jpeg)

### BigInt field

This is a data type in databases used to store very large integer values. It is commonly used when the range of values needed exceeds the limits of the standard INT data type.

In most systems, an INT field typically supports values from -2,147,483,648 to 2,147,483,647, but a BIGINT field can handle a much larger range. It ensures you can store these values without overflow errors.

This is useful when working with large datasets e.g. financial records, large IDs, or when performing mathematical computations that result in large numbers.
