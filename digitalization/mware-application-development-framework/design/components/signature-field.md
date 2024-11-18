---
description: >-
  Form fields that allow users to draw in a signature using mouse or phone
  inputs.
---

# Signature Field

#### About

`Signature` components can be used inside `Form` blocks in order to capture a signature from the user.

![](https://files.readme.io/d1915a9-image.png)

A `Signature` component will automatically populate when a `Form` block is used with a data source that includes a signature field. The Signature type can be added to columns in tables.

#### Manually Building Forms

When building forms manually, start by adding the `Form` component and selecting the schema of the table you wish to save a signature to, then add a `Signature` component. Your `Signature` component will display only once you have chosen an option from the dropdown list or manually assigned a value for `Field` by typing in the input field. Items in the dropdown list will populate from your schema (if you have selected one) if they are signature columns in the table.

![](https://files.readme.io/5edf797-image.png)

<figure><img src="https://files.readme.io/52d3c7b-image.png" alt="How to find the signature component"><figcaption><p>How to find the signature component</p></figcaption></figure>

####

Once published, clicking the signature field presents a pop-over in which the user can use their mouse to click-and-drag to create a signature. Clicking confirm closes the pop-over and displays the user's signature as part of the form. Signatures can also be added in the `Data` tab.

![](https://files.readme.io/556ce8f-image.png)

#### General Settings

| General Settings |                                                                                                                                                                |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Field            | Start typing or select from the dropdown options. If you have selected a schema that contains columns with a `signature`type, they will populate the dropdown. |
| Label            | Title displayed above the signature field in the form                                                                                                          |
| Help Text        | Helpful tip displayed underneath the field                                                                                                                     |
| Disabled         | Checkbox that disables the functionality of this component (reflected in a change of appearance).                                                              |
| On change        | Allows configurable actions to be set. These actions will run when the signature pop-over is closed, and also when an existing signature is removed.           |
| Validation       | Used to specify when a signature is required, and will show a configurable message under the field (defaults to "Error" if left blank")                        |
