# Using Forms with Objects

Liferay objects are integrated with the Forms application, so you can design forms for receiving user input and creating object entries. To do this, first add a new form and configure it to use the objects storage type. Then select the desired object, design your form, and map its fields to the selected object’s fields.

NOTE

All Forms features are supported for objects, including rules and validations. However, users cannot map object relationships to form fields. See [Designing Object Layouts](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/creating-and-managing-objects/layouts/designing-object-layouts) for how to create custom layouts that include object relationships.

### [Adding a Form for an Object](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-forms-with-objects#adding-a-form-for-an-object) <a href="#adding-a-form-for-an-object" id="adding-a-form-for-an-object"></a>

Follow these steps to add a form for creating object entries:

1. Navigate to the desired Site, open the _Site Menu_ (![Site Menu](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-menu.png)), and go to _Content & Data_ → _Forms_.
2. Click the _Add_ button (![Add Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-add.png)).
3. Click the _Configuration_ button (![Configuration Button](https://resources.learn.liferay.com/images/dxp/latest/en/images/icon-cog.png)) for the new form.
4.  Select the _Object_ storage type and the object you want to use with the form.

    This configures the form to store data for the selected object.

    ![Select the Object storage type and the desired object.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-forms-with-objects/images/01.png)

    NOTE

    Only published, active objects appear. You can only connect a form to one object at a time, though you can connect objects to multiple forms.
5. Click _Done_.

Once linked to the desired object, you can add field elements to the form and map them to the object’s fields.

### [Mapping Form Fields to Object Fields](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-forms-with-objects#mapping-form-fields-to-object-fields) <a href="#mapping-form-fields-to-object-fields" id="mapping-form-fields-to-object-fields"></a>

Follow these steps to map a form field to an object field.

1.  Add a form field that is compatible with the desired object field.

    See [Field Compatibility Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-forms-with-objects#field-compatibility-reference) for a list of which form fields are compatible with each type of object field.
2. Click the _Advanced_ tab.
3.  Select the desired _object field_.

    WARNING

    When mapped to a required object field, the form field is marked required automatically. If you’re [using a form rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/form-rules-overview) to [hide a required field conditionally](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/example-using-hidden-fields-with-form-rules), you must ensure it contains appropriate data or form submission fails. Using a [predefined value](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/creating-and-managing-forms/providing-help-text-and-predefined-values) with an [autofill rule](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms/form-rules/using-the-autofill-rule) can help ensure that a required hidden field is populated properly.

    ![In the Advanced tab, select the desired object field.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-forms-with-objects/images/02.png)
4.  Repeat the above process until all desired object fields are mapped to a form field.

    NOTE

    An object field can only be mapped to one form field.

    ![Map all desired object fields to the form fields.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-forms-with-objects/images/03.png)
5. Design the form layout. See [Forms](https://learn.liferay.com/web/guest/w/dxp/process-automation/forms) documentation to learn more about this process.
6.  Once you’ve finished designing the form, click _Publish_ to finish form creation.

    IMPORTANT

    Before you can publish an object form, it must fulfill all [mapping requirements](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-forms-with-objects#mapping-requirements).

After publishing, the form can be used to create object entries for the selected object.

![Use the published form to create object entries.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-forms-with-objects/images/04.png)

### [Mapping Requirements](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-forms-with-objects#mapping-requirements) <a href="#mapping-requirements" id="mapping-requirements"></a>

All forms with the Objects storage type have the following requirements.

* All form fields must map to an object field to prevent data loss.
* All required object fields must be mapped to a form field.

Attempting to publish a form with invalid mappings displays an error message that identifies what’s wrong.

![Attempting to publish a form with invalid mappings displays an error message.](https://resources.learn.liferay.com/images/dxp/latest/en/building-applications/objects/using-forms-with-objects/images/05.png)

### [Field Compatibility Reference](https://learn.liferay.com/web/guest/w/dxp/building-applications/objects/using-forms-with-objects#field-compatibility-reference) <a href="#field-compatibility-reference" id="field-compatibility-reference"></a>

| Object Field Types                          | Compatible Form Fields                |
| ------------------------------------------- | ------------------------------------- |
| Attachment                                  | ❌                                     |
| Boolean                                     | Boolean (checkbox)                    |
| Date                                        | Date                                  |
| Decimal (_previously Double_)               | Numeric → Decimal                     |
| Integer                                     | Numeric → Integer                     |
| Long Integer (_previously Long_)            | Numeric → Integer                     |
| Long Text (_previously Clob_)               | Text                                  |
| Picklist                                    | Select from List, Single Selection    |
| Precision Decimal (_previously BigDecimal_) | Numeric → Decimal                     |
| Relationship                                | ❌                                     |
| Rich Text                                   | Rich Text                             |
| Text (_previously String_)                  | Text, Multiple Selection, Grid, Color |
